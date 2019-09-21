---
layout: post
title: "C++: Round Robin CPU Scheduling Algorithm"
date: 2019-09-20
---

Round-robin (RR) is one of the algorithms employed by process and network schedulers in computing. As the term is generally used, time slices (also known as time quanta) are assigned to each process in equal portions and in circular order, handling all processes without priority (also known as cyclic executive). Round-robin scheduling is simple, easy to implement, and starvation-free. Round-robin scheduling can also be applied to other scheduling problems, such as data packet scheduling in computer networks. It is an operating system concept.

The name of the algorithm comes from the round-robin principle known from other fields, where each person takes an equal share of something in turn.

To schedule processes fairly, a round-robin scheduler generally employs time-sharing, giving each job a time slot or quantum (its allowance of CPU time), and interrupting the job if it is not completed by then. The job is resumed next time a time slot is assigned to that process. If the process terminates or changes its state to waiting during its attributed time quantum, the scheduler selects the first process in the ready queue to execute. In the absence of time-sharing, or if the quanta were large relative to the sizes of the jobs, a process that produced large jobs would be favoured over other processes.

Round-robin algorithm is a pre-emptive algorithm as the scheduler forces the process out of the CPU once the time quota expires. [Wikipedia](https://en.wikipedia.org/wiki/Round-robin_scheduling)

Here we have used some features of C++11. [Algorithm Library](https://en.cppreference.com/w/cpp/algorithm) will help to understand them.

Data members and member functions which are common to all scheduling algorithm are declared in `scheduling.h` and member functions are declared in `scheduling.cpp` . `virtual` keyword is used before `calcEndTime()` because each scheduling algorithm has different algorithm to calculate end time.

Turnaround time and waiting time are calculated using these formulas. 
     `turnAroundTime = endTime - arrivalTime`
     `waitingTime = turnAroundTime - burstTime`
     
Here is the C++ implementation    

scheduling.h
{% highlight c++ %}
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
{% endhighlight %}

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

roundrobin.h
```cpp
#ifndef ROUNDROBIN_H_
#define ROUNDROBIN_H_

#include "scheduling.h"

class RoundRobin : public Scheduler
{
    unsigned timeQuantum;
  public:
    RoundRobin(unsigned num, unsigned quantum);
    RoundRobin() = default;
    RoundRobin(const RoundRobin&)            = delete;
    RoundRobin &operator=(const RoundRobin&) = delete;
    RoundRobin(RoundRobin&&)                 = delete;
    RoundRobin &operator=(RoundRobin&&)      = delete;
    ~RoundRobin()                            = default;

    void calcEndTime();
};

#endif
```
roundrobin.cpp
```cpp
#include <iostream>
#include <vector>
#include <algorithm> // std::find
#include <iterator> // std::begin, std::end
#include <limits> //std::numeric_limits
#include "scheduling.h"
#include "roundrobin.h"

RoundRobin::RoundRobin(unsigned num, unsigned quantum): Scheduler(num)
{
    timeQuantum = quantum;
}

void RoundRobin::calcEndTime()
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
    while (!(std::all_of(burstTimeCopy.begin(), burstTimeCopy.end(),
            [] (unsigned e) { return e == 0; })))
    {
        unsigned currActiveProcessID = 0;
        auto it = burstTimeCopy.begin();
        while (it != burstTimeCopy.end())
        {
            if (burstTimeCopy[currActiveProcessID] > timeQuantum)
            {
                burstTimeCopy[currActiveProcessID] -= timeQuantum;
                timeCounter += timeQuantum;
            }
            else if (burstTimeCopy[currActiveProcessID] > 0)
            {
                timeCounter += burstTimeCopy[currActiveProcessID];
                burstTimeCopy[currActiveProcessID] = 0;
                endTime[currActiveProcessID] = timeCounter;
            }
            currActiveProcessID++;
            it++;
        }
    }
}

int main()
{
    unsigned num, timeQuantum;
    std::cout << "Enter number of process: ";
    std::cin >> num;
    std::cout << "\nEnter time quantum : ";
    std::cin >> timeQuantum;
    RoundRobin roundRobin(num, timeQuantum);
    roundRobin.calcEndTime();
    roundRobin.calcTurnAroundTime();
    roundRobin.calcWaitingTime();
    roundRobin.printInfo();
}
```

You can compile with this command in Linux `g++ -std=c++11 roundrobin.cpp scheduling.cpp`  
      
      
