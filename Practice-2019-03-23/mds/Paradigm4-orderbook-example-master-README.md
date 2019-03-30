orderbook-example
============

Example custom aggregation function that computes a basic order book to a fixed
depth from example sample market book data (see example below).
The depth is fixed at 10 at compiled time (see the source to change the depth).

### Compile & install the custom aggregation operator

The orderbook custom aggregate function is defined in
orderbook/src/orderbook.cpp. You'll need to have installed a copy of SciDB and
its development packages. You can install SciDB's development packages with (shown
for SciDB version 14.3, adjust version numbers accordingly for whatever version
of SciDB you're running).

#### Installation of SciDB devel packages on RHEL 6 or CentOS
```
sudo yum install scidb-14.3-dev.x86_64 scidb-14.3-dev-tools.x86_64 scidb-14.3-dev-tools-dbg.x86_64 scidb-14.3-plugins-dbg.x86_64 scidb-14.3-libboost-devel.x86_64 scidb-14.3-libboost-static.x86_64 libpqxx-devel
```
#### Installation of SciDB devel packages on Ubuntu
```
sudo apt-get install scidb-14.3-dev scidb-14.3-dev-tools scidb-14.3-libboost1.54-dev scidb-14.3-libmpich2-dev scidb-14.3-libboost1.54-all-dev libpqxx3-dev liblog4cxx10-dev

# For SciDB 14.12, for example:
sudo apt-get install scidb-14.12-dev scidb-14.12-dev-tools scidb-14.12-libboost1.54-dev scidb-14.12-libmpich2-dev scidb-14.12-libboost1.54-all-dev libpqxx3-dev liblog4cxx10-dev

```

Build and install the order book aggregate with:
```
# Clone the git repository
git clone https://github.com/Paradigm4/orderbook-example

# (If that didn't work, maybe you don't have git installed? You could alternatively try:
# wget https://github.com/Paradigm4/orderbook-example/archive/master.zip
# unzip master.zip
# mv orderbook-example-master orderbook-example

cd orderbook-example
make
cp liborderbook.so /opt/scidb/14.3/lib/scidb/plugins
# (If that fails, try as root:)
# sudocp liborderbook.so /opt/scidb/14.3/lib/scidb/plugins
```
Adjusting your target directory accordingly for your installed version of SciDB.
Be sure to copy liborderbook.so to the plugin directory on all your SciDB cluster nodes.


## Synopsis

The orderbook aggregate function normally runs with SciDB's `cumulate`
aggregate operator along a time dimension:

```
cumulate(arca, orderbook(order_record), ms)
```

where,

* arca  is a time by symbol data matrix,
* ms    is the name of the time dimension,
* order&#95;record is a specially formatted string input attribute for orderbook.

The orderbook function expects the input attribute to have the following special string form:
```
order_type, ref_id, price, size, symbol, ask_bid_type|
```
(the string *must* have a trailing vertical bar.)

The output is a 2-d array with the same dimensions as the input
array and a single string attribute with the format
```
bp1, bv1, bp2, bv2, bp3, bv3 |  ap1, av1, ap2, av2, ap3, av3
```
where, bp1 means "bid price 1", "bv1 means bid volume 1", and
ap1 and av1 are similarly defined for ask prices and volumes, and
bp1 &lt; bp2 &lt; bp3 &lt; ap1 &lt; ap2 &lt; ap3.

It's possible (likely near the start of the day) that one or more
prices and volumes are not available, in which case the book depths
on the bid and ask side may differ.

See the shell scripts in this project for examples of splitting this
output string up into separate attributes.


## Example Use

### Download raw sample ARCA data into SciDB (about 2.3 GB)
```
wget ftp://ftp.nyxdata.com/Historical%20Data%20Samples/TAQ%20NYSE%20ArcaBook/EQY_US_ALL_ARCA_BOOK_20130404.csv.gz

./load.sh EQY_US_ALL_ARCA_BOOK_20130404.csv.gz
```
Note! The example is a sizable download (2.3 GB) and can take a while to load on small
SciDB clusters.


The raw data contains three different record formats corresponding to the
three different order types: A)dd, M)odify, & D)elete.  The load makes
three passes through the raw data file, one per order type, and standardizes
all three record types into a common format. The `load.sh` script standardizes records, storing
them into a single array named flat.

The output array from this step is called flat.


### Redimension into a 2D array organized by time and symbol
```
./redim.sh
```
Since SciDB requires integer dimensions, the symbol dimension is first mapped
to an integer. This script then appends a new attribute named order&#95;record to
each row in the arca&#95;flat array. This attribute is the input value required by
the orderbook aggregate, described in step (1) above. 

The output array from this step is called symbol&#95;time.


### Run book.sh

This produces the order book, formatted as the following comma separated output: 
       {symbol, timestamp} 'bid price, bid volume, ask price, ask volume'

The output array is named book.

