# React v Angular performance comparison

#### tl;dr

Based off our investigation, we can make the following conclusions:

1. __Scripting__: Angular spends less time Scripting than React

2. __Memory__: Angular utilizes less Heap with less memory volatility. 
We've also observed that running Angular results in fewer Major GCs.

3. __Visuals__: Angular processes updates more quickly from the time they are generated

#### Demo
* [Angular](https://scottweinstein.github.io/react-ng-perf-comparison/ang/)
* [React v15](https://scottweinstein.github.io/react-ng-perf-comparison/react/)
* [React v16](https://scottweinstein.github.io/react-ng-perf-comparison/react-16/)

ToC
=================

* [Overview](#overview)
* [Procedure](#procedure)
* [Results](#results)
	* [Environment](#environment)
	* [Test Approach](#test-approach)
	* [Findings](#findings)
		* [Scripting](#scripting)
			* [Summary Table](#summary-table)
			* [Test 1: 500 Components, 50ms updates](#test-1-500-components-50ms-updates)
			* [Test 2: 100 Components, 50ms updates](#test-2-100-components-50ms-updates)
		* [Memory](#memory)
			* [React utilizes larger heap sizes and significantly greater quantity of Major GCs](#react-utilizes-larger-heap-sizes-and-significantly-greater-quantity-of-major-gcs)
			* [React GC Patterns and larger scripting time #2](#react-gc-patterns-and-larger-scripting-time-2)
		* [Noticeable delay in data freshness on the UI](#noticeable-delay-in-data-freshness)

# Overview

This repo contains 2 projects with feature parity, one based on React v15 and one based 
on Angular 4. Aside from RxJS, there are no additional libraries. The Angular app was 
generated with the [Angular CLI](https://github.com/angular/angular-cli) and the React
 app was generated with [create-react-app](https://github.com/facebookincubator/create-react-app). 
Performance Results were calculated for each using optimized production builds.

The purpose of these 2 projects is to compare Angular and React rendering performance in 
scenarios where there are many DOM elements updating extremely quickly, which is a common 
scenario in industries consuming real-time data feeds.
 
Each app instantiates a preset number of simple components (configured through 
`initialQuantity` property in [app.component.ts](./ang/src/app/app.component.ts#L12) and
 [App.js](./react/src/App.js#L9)). Each component kicks off an RxJS `Observable` data stream 
 that publishes every 50ms. Each component contains 2 bindings that update on each RxJS emit, 
 where the first binding represents the time interval in ms from the previous update to the 
 current and the 2nd binding contains the updated data it received from the `Observable`. 

# Procedure

1. Close as many open apps as possible on the machine to prevent possible
 resource contention (CPU, Memory, I/O, ...) that could compromise test results.
1. Prepare a fresh Chrome: Disable any relevant extensions, clear caches, close all other
 tabs/windows, etc.
1. Run `npm install` from both Angular and React app project roots.
1. Set the `initialQuantity` property in both [app.component.ts](./ang/src/app/app.component.ts#L12) 
and [App.js](./react/src/App.js#L9)) to the desired number of controls you'd like to test.
1. Run production optimized builds of both apps from each project root `npm run build`
1. Serve up the generated assets using your favorite http-server for each project (one 
project at a time). I use `npm install -g serve` `serve -s build`
1. We are focused on update speed and not initial load so wait until the DOM loads and
 then run the Performance Test from Chrome Developer Tools for your test duration.
1. For each test run, record the exact scripting time and total test duration.
1. Make sure to start fresh for each test, close the page, clear the cache, clear the 
performance profiler.

# Results

We currently include results both stats and screenshots for 20 test runs found under 
[test-results](./test-results) 

## Environment
- macOS Sierra (10.12.5)
- Chrome 58.0.3029.110 (64-bit)
- MacBook Pro (Retina, Mid 2012) 2.6 GHz i7
- 512gb SSD
- 16 GB DDR3
- nVidia GeForce GT 650M 1024 MB

## Test Approach
- Test 1: 500 Components, 50 ms updates, ~30 seconds
- Test 2: 100 Components, 50 ms updates, ~30 seconds
- 5 Runs of each test per framework
- Take screenshot of profile output.
- Record total test duration, scripting duration, and percentage of total test duration 
spent Scripting. We use the percentage of the total time instead of an absolute number 
since the tooling makes it difficult to set an precise test duration. 

## Findings

We have identified 3 separate areas of concern regarding React performance:
1. Scripting
1. Memory
1. Noticable delay in data freshness on the UI 

### Scripting

Angular spends a smaller percentage of time scripting than React. 

__For Test 1 (500 Components), we found that React averaged 21% more time scripting than Angular__. 

__For Test 2 (100 Components), we found that React averaged 92% more time scripting than Angular.__

#### Summary Table

| Components | Library | Avg - % of Test Time spent Scripting | Avg - Scripting Time (ms) | StdDev - % of Test Time spent Scripting |
| --- | --- | --- | ---| --- | 
| 100 | Angular | 6.3% | 1,905 | 0.3% |
| 100 | React	| 12.1%	&#x1F53A; | 3,669 | 0.5% | 
| 100 | React v16 Alpha | 11.6% | 3,547 | 1.2% | 
|   |
| 500 | Angular	| 44.5%	| 13,594 | 0.5% |
| 500 | React | 53.9% | 16,494 | 2.1% |
| 500 | React v16 Alpha | 58.7% &#x1F53A; | 17,803 | 1.3% |

*See the [Test Results folder](./test-results/) for screenshots of the entire result set.*


### Test 1: 500 Components, 50ms updates

| TestId | Type | Components | Run # | Total Duration (ms) | Scripting (ms) | % of Test Time Spent Scripting |
| --- | --- | --- | --- | --- | --- | --- |
| 01 [(img)](/../../raw/master/test-results/01.png) | React v15 | 500 | 1 | 30,452 | 16,727 | 54.9% |
| 02 [(img)](/../../raw/master/test-results/02.png) | React v15 | 500 | 2 | 30,871 | 17,609 | 57.0% |
| 03 [(img)](/../../raw/master/test-results/03.png) | React v15 | 500 | 3 | 30,862 | 15,893 | 51.5% |
| 04 [(img)](/../../raw/master/test-results/04.png) | React v15 | 500 | 4 | 30,348 | 16,087 | 53.0% |
| 05 [(img)](/../../raw/master/test-results/05.png) | React v15 | 500 | 5 | 30,512 | 16,154 | 52.9% |
|  | 	                                                
| 21 [(img)](/../../raw/master/test-results/21.png) | React v16 | 500 | 1 | 30,369 | 17,886 | 58.9% |
| 22 [(img)](/../../raw/master/test-results/22.png) | React v16 | 500 | 2 | 30,434 | 18,472 | 60.7% |
| 23 [(img)](/../../raw/master/test-results/23.png) | React v16 | 500 | 3 | 30,040 | 17,618 | 58.6% |
| 24 [(img)](/../../raw/master/test-results/24.png) | React v16 | 500 | 4 | 30,312 | 17,311 | 57.1% |
| 25 [(img)](/../../raw/master/test-results/25.png) | React v16 | 500 | 5 | 30,392 | 17,730 | 58.3% |
|  | 	                                                
| 11 [(img)](/../../raw/master/test-results/11.png) | Angular | 500 | 1 | 30,451 | 13,750 | 45.2% |
| 12 [(img)](/../../raw/master/test-results/12.png) | Angular | 500 | 2 | 30,611 | 13,592 | 44.4% |
| 13 [(img)](/../../raw/master/test-results/13.png) | Angular | 500 | 3 | 31,021 | 13,694 | 44.1% |
| 14 [(img)](/../../raw/master/test-results/14.png) | Angular | 500 | 4 | 30,383 | 13,583 | 44.7% |
| 15 [(img)](/../../raw/master/test-results/15.png) | Angular | 500 | 5 | 30,369 | 13,349 | 44.0% |

### Test 2: 100 Components, 50ms updates

| TestId | Type | Components | Run # | Total Duration (ms) | Scripting (ms) | % of Test Time Spent Scripting |
| ------ | ----- | --------- | ----- | ------------------- | -------------- | ------------------------------ |
| 06 [(img)](/../../raw/master/test-results/06.png) | React v15 | 100 | 1 | 30,570 | 3,895 | 12.7% |
| 07 [(img)](/../../raw/master/test-results/07.png) | React v15 | 100 | 2 | 29,890 | 3,564 | 11.9% |
| 08 [(img)](/../../raw/master/test-results/08.png) | React v15 | 100 | 3 | 30,615 | 3,575 | 11.7% |
| 09 [(img)](/../../raw/master/test-results/09.png) | React v15 | 100 | 4 | 30,486 | 3,564 | 11.7% |
| 10 [(img)](/../../raw/master/test-results/10.png) | React v15 | 100 | 5 | 30,271 | 3,745 | 12.4% |
| |
| 26 [(img)](/../../raw/master/test-results/26.png) | React v16 | 100 | 1 | 30,391 | 3,963 | 13.0% |
| 27 [(img)](/../../raw/master/test-results/27.png) | React v16 | 100 | 2 | 30,436 | 3,146 | 10.3% |
| 28 [(img)](/../../raw/master/test-results/28.png) | React v16 | 100 | 3 | 30,499 | 3,176 | 10.4% |
| 29 [(img)](/../../raw/master/test-results/29.png) | React v16 | 100 | 4 | 30,414 | 3,604 | 11.8% |
| 30 [(img)](/../../raw/master/test-results/30.png) | React v16 | 100 | 5 | 30,722 | 3,848 | 12.5% |
| |
| 16 [(img)](/../../raw/master/test-results/16.png) | Angular | 100 | 1 | 30,084 | 1,986 | 6.6% |
| 17 [(img)](/../../raw/master/test-results/17.png) | Angular | 100 | 2 | 29,801 | 1,915 | 6.4% |
| 18 [(img)](/../../raw/master/test-results/18.png) | Angular | 100 | 3 | 30,257 | 1,956 | 6.5% |
| 19 [(img)](/../../raw/master/test-results/19.png) | Angular | 100 | 4 | 30,292 | 1,817 | 6.0% |
| 20 [(img)](/../../raw/master/test-results/20.png) | Angular | 100 | 5 | 30,406 | 1,851 | 6.1% |

### Example: Running React results in 3 additional seconds of scripting in a 60 second test
![React Major GC Memory Usage 1](/../../raw/master/test-results/100-comps-60-sec-both-cpu-memory-run1.png)

---

### Memory

#### React utilizes larger heap sizes and significantly greater quantity of Major GCs
![React Major GC Memory Usage](/../../raw/master/test-results/100-comps-60-sec-both-memory-profile.png)

#### React GC Patterns and larger scripting time #2
![React Major GC Memory Usage 2](/../../raw/master/test-results/100-comps-60-sec-both-cpu-memory-run2.png)

---

### Noticeable delay in data freshness

Using RxJS timeInterval() as a simple visual representation of throughput, Angular 
components consistently show lower time intervals than React. Due to the single-threaded 
nature of JavaScript, the RxJS timeInterval (1st binding) is a simple visual 
representation of how quickly each framework can process a data update given a consistent
number of microtasks on the event loop - the closer it is to the publisher's 50ms 
rate, the better.

**Note that while this screenshot displays only a small lag through the time interval
 field, that lag is noticably amplified in a complex enterprise application where the 
 event loop is further bogged down by many factors including UI component complexity, 
 streaming data serialization/deserialization, and DOM events.**

React on left. Note that the time interval for nearly all 100 React components display 
a time interval of 59 or above, while the Angular components are consistently lower 
and are closer to the 50ms rate that RxJS is configured to publish.
![Intervals clearly higher in React app](/../../raw/master/test-results/100-comps-ng-vs-react-perf.png)

