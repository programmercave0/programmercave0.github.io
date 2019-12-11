---
layout: post
title: "C++: Rock Paper Scissor Simple Console Implementation"
subtitle: "Rock–paper–scissors is often used as a fair choosing method between two people, similar to coin flipping, drawing straws, or throwing dice in order to settle a dispute or make an unbiased group decision. "
author: "Programmercave"
tags:  [Cpp, Games]
date: 2018-06-18
---

**Rock–Paper–Scissors** (also known as paper, scissors, stone or other variants) is a hand game usually played between two people, in which each player simultaneously forms one of three shapes with an outstretched hand. These shapes are "rock" (a closed fist), "paper" (a flat hand), and "scissors" (a fist with the index and middle fingers extended, forming a V). "Scissors" is identical to the two-fingered V sign (aka "victory" or "peace sign") except that it is pointed horizontally instead of being held upright in the air. A simultaneous, zero-sum game, it has only two possible outcomes: a draw, or a win for one player and a loss for the other. 

Rock–paper–scissors is often used as a fair choosing method between two people, similar to coin flipping, drawing straws, or throwing dice in order to settle a dispute or make an unbiased group decision. Unlike truly random selection methods, however, rock–paper–scissors can be played with a degree of skill by recognizing and exploiting non-random behavior in opponents.[Wikipedia](https://en.wikipedia.org/wiki/Rock%E2%80%93paper%E2%80%93scissors)

Here is the C++ implementation 

game_control.h

```cpp
#ifndef GAME_CONTROL_H
#define GAME_CONTROL_H

#include <iostream>
#include <array>
#include <string>

class Game
{
    unsigned int number_of_turns = 0;
    unsigned int user_score = 0, comp_score = 0;
    std::array<std::string, 3> choices = {{"ROCK", "PAPER", "SCISSOR"}};
    std::string user_name;

  public:
    Game()  = default;
    ~Game() = default;

    void start_menu();
    void start_game();
    void game_play(unsigned int, unsigned int);
    void exit_game();
};

#endif
```

game_control.cpp

```cpp
#include <iostream>
#include <string>
#include <cstdlib>
#include <ctime>

#include "game_control.h"


void Game::start_menu()
{
    std::string banner = R"x(
    ####  ####  ####  #  #   ####  ####  ####  ####  ####
    #  #  #  #  #     # #    #  #  #  #  #  #  #     #  #
    ####  #  #  #     ##     ####  ####  ####  ####  ####
    # #   #  #  #     # #    #     #  #  #     #     # #
    #  #  ####  ####  #  #   #     #  #  #     ####  #  #


       ####  ####  #  ####  ####  ####  ####
       #     #     #  #     #     #  #  #  #
       ####  #     #  ####  ####  #  #  ####
          #  #     #     #     #  #  #  # #
       ####  ####  #  ####  ####  ####  #  #

    )x";

    std::cout << banner;

    unsigned int selection;
    show_menu:
    std::cout << "\tPress\n";
    std::cout << "\t1 to Enter\n";
    std::cout << "\t2 to Exit\n";
    std::cin >> selection;

    std::cin.clear();
    std::cin.ignore();

    switch(selection)
    {
        case 1: start_game();
                break;
        case 2: exit_game();
                break;
        default: std::cout << "-------Enter valid choice--------\n";
                 goto show_menu;
                 break;
    }
}

void Game::exit_game()
{
    std::cout << "Thanks for playing!!\n";
    exit(EXIT_SUCCESS);
}

void Game::start_game()
{

    std::cout << "\nEnter your name\n";
    std::getline(std::cin, user_name);
    std::cout << "Enter number of turns for you want to play\n";
    std::cin >> number_of_turns;

    while (number_of_turns--)
    {
        unsigned int user_choice, comp_choice;
        std::cout << "\nEnter your choice\n";
        show_choices:
        std::cout << "\nPress\n";
        std::cout << "1 for ROCK\t2 for PAPER\t3 for SCISSOR\n";
        std::cin >> user_choice;
        if (user_choice > 3)
        {
            std::cout << "\n---------Please enter valid choice------------\n";
            goto show_choices;
        }
        user_choice--;

        srand(time(0));
        comp_choice = rand() % 3;

        game_play(user_choice, comp_choice);
        std::cout << "The score is\t";
        std::cout << user_name << " = " << user_score;
        std::cout << "\tComputer = " << comp_score << "\n";
    }
    exit_game();
}

void Game::game_play(unsigned int user_choice, unsigned int comp_choice)
{
    if (user_choice != comp_choice)
    {
        std::cout << user_name << " chooses " << choices[user_choice];
        std::cout << " and Computer chooses " << choices[comp_choice] << "\n";

        if ( (user_choice % 3) == ((comp_choice+1) % 3) )
        {
            std::cout << "----" << user_name << " Wins!!----\n";
            user_score++;
        }
        else
        {
            std::cout << "----Computer Wins!!----\n";
            comp_score++;
        }
    }
    else
    {
        std::cout << "Both " << user_name << " and Computer chooses " << choices[user_choice];
        std::cout << "\n";
    }
}
```

main.cpp

```cpp
#include "game_control.h"

int main()
{
    Game game;
    game.start_menu();
    game.start_game();
}
```

Makefile

```
CC = g++
CFLAGSS = -Wall -std=c++11
SOURCES = main.cpp game_control.cpp
EXECUTABLE = main

all:
	$(CC) $(CFLAGSS) $(SOURCES) -o $(EXECUTABLE)
```


 
 
 
