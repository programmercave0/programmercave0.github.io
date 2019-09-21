---
layout: post
title: "C++: Shortest Job First Preemptive CPU Scheduling Algorithm"
tags:  C++, Algorithm, Operating System
date: 2018-07-04
---

Shortest job first (SJF), also known as shortest job next (SJN) or shortest process next (SPN), is a scheduling policy that selects for execution the waiting process with the smallest execution time. SJN is a non-preemptive algorithm. Shortest remaining time is a preemptive variant of SJN.

Shortest job next is advantageous because of its simplicity and because it minimizes the average amount of time each process has to wait until its execution is complete. However, it has the potential for process starvation for processes which will require a long time to complete if short processes are continually added. Highest response ratio next is similar but provides a solution to this problem using a technique called aging.

Another disadvantage of using shortest job next is that the total execution time of a job must be known before execution. While it is impossible to predict execution time perfectly, several methods can be used to estimate it, such as a weighted average of previous execution times.

Shortest job next can be effectively used with interactive processes which generally follow a pattern of alternating between waiting for a command and executing it. If the execution burst of a process is regarded as a separate "job", past behaviour can indicate which process to run next, based on an estimate of its running time.

Shortest job next is used in specialized environments where accurate estimates of running time are available. [Wikipedia]

Here we have used some features of C++11. [Algorithm Library](https://en.cppreference.com/w/cpp/algorithm) will help to understand them.

Data members and member functions which are common to all scheduling algorithm are declared in `scheduling.h` and member functions are declared in `scheduling.cpp` . `virtual` keyword is used before `calcEndTime()` because each scheduling algorithm has different algorithm to calculate end time.

Turnaround time and waiting time are calculated using these formulas.


    `turnAroundTime = endTime - arrivalTime`
    `waitingTime = turnAroundTime - burstTime `


 Here is the C++ implementation
 
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

shortestjobfirst.h

```cpp
#ifndef SHORTESTJOBFIRST_H_
#define SHORTESTJOBFIRST_H_

#include "scheduling.h"

class ShortestJobFirst : public Scheduler
{
  public:
    ShortestJobFirst(unsigned num);
    ShortestJobFirst() = default;
    ShortestJobFirst(const ShortestJobFirst&)            = delete;
    ShortestJobFirst &operator=(const ShortestJobFirst&) = delete;
    ShortestJobFirst(ShortestJobFirst&&)                 = delete;
    ShortestJobFirst &operator=(ShortestJobFirst&&)      = delete;
    ~ShortestJobFirst()                                  = default;

    void calcEndTime();
};

#endif
```

shortestjobfirst.cpp

```cpp
#include <iostream>
#include <array>
#include <vector>
#include <algorithm> // std::find
#include <iterator> // std::begin, std::end
#include <limits> //std::numeric_limits
#include "scheduling.h"
#include "shortestjobfirst.h"


ShortestJobFirst::ShortestJobFirst(unsigned num) :Scheduler(num)
                                                  {}

void ShortestJobFirst::calcEndTime()
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
        if (timeCounter <= data[dataSize -1].arrivalTime)
        {
            unsigned minBurstTime = std::numeric_limits<uint>::max();
            //Find index with minimum burst Time remaining
            for (std::size_t i = 0; i < burstTimeCopy.size(); i++)
            {
                if (burstTimeCopy[i] != 0 && burstTimeCopy[i] < minBurstTime
                  && data[i].arrivalTime <= timeCounter)
                {
                    minBurstTime = burstTimeCopy[i];
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
            unsigned minBurstTime = std::numeric_limits<uint>::max();
            //Find index with minimum burst Time remaining
            for (std::size_t i = 0; i < burstTimeCopy.size(); i++)
            {
                if (burstTimeCopy[i] != 0 && burstTimeCopy[i] < minBurstTime)
                {
                    minBurstTime = burstTimeCopy[i];
                    currActiveProcessID = i;
                }
            }
            timeCounter += minBurstTime;
            endTime[currActiveProcessID] = timeCounter;
            burstTimeCopy[currActiveProcessID] = 0;
        }
    }
}

int main()
{
    int num;
    std::cout << "Enter the number of processes\n";
    std::cin >> num;
    ShortestJobFirst batch(num);
    batch.calcEndTime();
    batch.calcTurnAroundTime();
    batch.calcWaitingTime();
    batch.printInfo();
}
```

You can compile with this command in Linux `g++ -std=c++11 shortestjobfirst.cpp scheduling.cpp`
