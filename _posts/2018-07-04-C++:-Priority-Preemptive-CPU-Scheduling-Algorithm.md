---
layout: post
title: "C++: Priority Preemptive CPU Scheduling Algorithm"
tags:  C++, Algorithm, Operating System
date: 2018-07-04
---

In Priority scheduling algorithm, a priority is associated with each process, and the CPU is allocated to the process with the highest priority. Equal-priority processes are scheduled in FCFS order

Priorities can be defined either internally or externally. Internally defined priorities use some measurable quantity or quantities to compute the priority of a process. For example, time limits, memory requirements, the number of open files, and the ratio of average I/O burst to average CPU burst have been used in computing priorities. External priorities are set by criteria outside the operating system, such as the importance of the process, the type and amount of funds being paid for computer use, the department sponsoring the work, and other, often political, factors.

A preemptive priority scheduling algorithm will preempt the CPU if the priority of the newly arrived process is higher than the priority of the currently running process. A nonpreemptive priority scheduling algorithm will simply put the new process at the head of the ready queue.

A major problem with priority scheduling algorithms is indefinite blocking, or starvation.

A solution to the problem of indefinite blockage of low-priority processes is aging. Aging involves gradually increasing the priority of processes that wait in the system for a long time. 

Here we have used some features of C++11. [Algorithm Library](https://en.cppreference.com/w/cpp/algorithm) will help to understand them.

Data members and member functions which are common to all scheduling algorithm are declared in `scheduling.h` and member functions are declared in `scheduling.cpp` . `virtual` keyword is used before `calcEndTime()` because each scheduling algorithm has different algorithm to calculate end time.

Turnaround time and waiting time are calculated using these formulas.
          turnAroundTime = endTime - arrivalTime
          waitingTime = turnAroundTime - burstTime 
          
Here is the C++ implementation. 

scheduling.h

```cpp
#ifndef SCHEDULING_H_
#define SCHEDULING_H_

#include <vector>

class Scheduler
{
  public:

    double avgWaitingTime;
    double avgTurnAroundTime;

    struct Data
    {
        unsigned arrivalTime;
        //When process start to execute
        unsigned burstTime;
        //only for priority Scheduling
        unsigned priority;

        Data(unsigned arrivalTime, unsigned burstTime, unsigned priority):
            arrivalTime(std::move(arrivalTime)),
            burstTime(std::move(burstTime)),
            priority(std::move(priority))
            {}
        Data() = default;
    };

    std::vector<Data> data;
    //process wait to execute after they have arrived
    std::vector<unsigned> waitingTime;
    //total time taken by processes
    std::vector<unsigned> turnAroundTime;
    //time when a process end
    std::vector<unsigned> endTime;

    Scheduler(unsigned num = 0);
    Scheduler(const Scheduler&)            = delete;
    Scheduler &operator=(const Scheduler&) = delete;
    Scheduler(Scheduler&&)                 = delete;
    Scheduler &operator=(Scheduler&&)      = delete;
    ~Scheduler()                           = default;

    void calcWaitingTime();
    void calcTurnAroundTime();
    virtual void calcEndTime() = 0;
    void printInfo() const;
};

#endif
```

scheduling.cpp

```cpp
#include <iostream>
#include <vector>
#include "scheduling.h"

Scheduler::Scheduler(unsigned num): endTime(num, 0)
{
    unsigned arrivalVal, burstVal, priorityVal;
    data.reserve(num);
    endTime.reserve(num);
    waitingTime.reserve(num);
    turnAroundTime.reserve(num);

    std::cout << "\nEnter arrival time and burst time and priority eg(5 18 2)\n";
    std::cout << "If it is not priority scheduling enter 0 for priority\n";
    std::cout << "Lower integer has higher priority\n";
    for (unsigned i = 0; i < num; i++)
    {
        std::cout << "\nProcess" << i+1 << ": ";
        std::cin >> arrivalVal >> burstVal >> priorityVal;
        data.push_back( Data(arrivalVal, burstVal, priorityVal) );
    }
}

void Scheduler::calcTurnAroundTime()
{
    double sum = 0.00;
    for (std::size_t i = 0; i < data.size(); i++)
    {
        unsigned val = endTime[i] - data[i].arrivalTime;
        turnAroundTime.push_back(val);
        sum += (double)val;
    }
    avgTurnAroundTime = sum / turnAroundTime.size();
}

void Scheduler::calcWaitingTime()
{
    double sum = 0.00;
    for (std::size_t i = 0; i < data.size(); i++)
    {
        unsigned val = turnAroundTime[i] - data[i].burstTime;
        waitingTime.push_back(val);
        sum += (double)val;
    }
    avgWaitingTime = sum / waitingTime.size();
}

void Scheduler::printInfo() const
{
    std::cout << "ProcessID\tArrival Time\tBurst Time\tPriority\tEnd Time\tWaiting Time";
    std::cout << "\tTurnaround Time\n";
    for (std::size_t i = 0; i < data.size(); i++)
    {
        std::cout << i+1 << "\t\t" << data[i].arrivalTime << "\t\t";
        std::cout << data[i].burstTime << "\t\t" <<data[i].priority << "\t\t";
        std::cout << endTime[i] << "\t\t";
        std::cout << waitingTime[i] <<"\t\t" << turnAroundTime[i] <<'\n';
    }
    std::cout << "Average Waiting Time : " << avgWaitingTime << '\n';
    std::cout << "Average Turn Around Time : " << avgTurnAroundTime << '\n';
}
```

priority.h

```cpp
#ifndef PRIORITY_H_
#define PRIORITY_H_

#include "scheduling.h"

class Priority : public Scheduler
{
  public:
    Priority(unsigned num);
    Priority() = default;
    Priority(const Priority&)            = delete;
    Priority &operator=(const Priority&) = delete;
    Priority(Priority&&)                 = delete;
    Priority &operator=(Priority&&)      = delete;
    ~Priority()                          = default;

    void calcEndTime();
};

#endif
```

priority.cpp

```cpp
#include <iostream>
#include <vector>
#include <algorithm> // std::find
#include <iterator> // std::begin, std::end
#include <limits> //std::numeric_limits
#include "scheduling.h"
#include "priority.h"

Priority::Priority(unsigned num): Scheduler(num)
                                  {}

void Priority::calcEndTime()
{
    //If arrival time is not sorted
    //sort burst time according to arrival time
    static const auto byArrival = [](const Data &a, const Data &b)
    {
        return a.arrivalTime < b.arrivalTime;
    };
    std::sort(data.begin(), data.end(), byArrival);

    //copy values of burst time in new vector
    std::vector<unsigned> burstTimeCopy;
    for (auto it = data.begin(); it != data.end(); ++it)
    {
        unsigned val = (*it).burstTime;
        burstTimeCopy.push_back(val);
    }

    unsigned timeCounter = 0;
    unsigned currActiveProcessID = 0;
    while (!(std::all_of(burstTimeCopy.begin(), burstTimeCopy.end(),
            [] (unsigned e) { return e == 0; })))
    {
        std::size_t dataSize = data.size();
        //All processes are not arrived
        if (timeCounter <= data[dataSize - 1].arrivalTime)
        {
            unsigned maxPriority = std::numeric_limits<uint>::max();
            for (std::size_t i = 0; i < burstTimeCopy.size(); i++)
            {
                if (burstTimeCopy[i] != 0 && data[i].priority < maxPriority
                    && data[i].arrivalTime <= timeCounter)
                {
                    maxPriority = data[i].priority;
                    currActiveProcessID = i;
                }
            }
            burstTimeCopy[currActiveProcessID] -= 1;
            timeCounter++;
            if (burstTimeCopy[currActiveProcessID] == 0)
            {
                endTime[currActiveProcessID] = timeCounter;
            }
        }
        else //When all processes are arrived
        {
            unsigned maxPriority = std::numeric_limits<uint>::max();
            for (std::size_t i = 0 ; i < burstTimeCopy.size(); i++)
            {
                if (burstTimeCopy[i] != 0 && data[i].priority < maxPriority)
                {
                    maxPriority = data[i].priority;
                    currActiveProcessID = i;
                }
            }
            timeCounter += burstTimeCopy[currActiveProcessID];
            burstTimeCopy[currActiveProcessID] = 0;
            endTime[currActiveProcessID] = timeCounter;
        }
    }
}

int main()
{
    int num;
    std::cout << "Enter the number of processes\n";
    std::cin >> num;
    Priority prioritySchedule(num);
    prioritySchedule.calcEndTime();
    prioritySchedule.calcTurnAroundTime();
    prioritySchedule.calcWaitingTime();
    prioritySchedule.printInfo();
}
```

You can compile with this command in Linux `g++ -std=c++11 priority.cpp scheduling.cpp`
