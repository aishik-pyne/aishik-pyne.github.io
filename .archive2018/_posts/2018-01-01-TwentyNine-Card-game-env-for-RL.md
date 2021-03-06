---
layout: post
title:  "Twenty Nine"
date:   2018-01-01
excerpt: "A Card Game environment for Reinforcement Learning"
image: "/images/twentynine.jpg"
comments: true
---
# TwentyNine

A Reinforcement Learning method to play the game of [TwentyNine](https://en.wikipedia.org/wiki/Twenty-eight_(card_game) "Wikipedia")

---
### Contents

- [Introduction](#introduction)
- [Environment](#environment)
- [Bidding Network](#bidding-network)
- [Gameplay Network](#gameplay-network)
- [References](#references)

---
<a name="introduction"></a>
### Introduction

Twenty-eight originated in India. The game is believed to be related to the European family of Jass card games, which originated in the Netherlands. These games are believed to have been brought to India by Indian South Africans who were also influenced by the Afrikaaner game of Klaverjas. The rules of game can be found [here](https://en.wikipedia.org/wiki/Twenty-eight_(card_game) "Wikipedia").

I would advice you to read the rules of game first if you haven't played the game in real life

---
<a name="environment"></a>
### Environment

I used [Pydealer](https://pypi.python.org/pypi/pydealer/ "PyDealer") module is used to handle stacks of cards.

4 cards are dealt to each player. The bidding starts from the player right to the dealer. The highest bidder sets the trump. Then 4 cards are dealt to each player again.

The game state space consists of
- Hands of all players : List of 4 `pydealer.Stack()`
- Current trick : A `pydealer.Stack()`
- Current player : `int`
- Trump suit: `pydealer.const.SUITS`
- Trump revealed: `bool`
- Team winning stack: List of 2 `pydealer.Stack()`
