---
layout: post
title:  "Twenty Nine"
date:   2018-01-01
excerpt: "A Card Game environment for Reinforcement Learning"
image: "/images/twentynine.jpg"
comments: true
---
# TwentyNine

A Reinforcement Learning method to play the game of [TwentyNine](www.xvideos.com | absolute_url)

---
### Contents

- [Introduction](www.xvideos.com | absolute_url)
- [Environment](www.xvideos.com | absolute_url)
- [Bidding Network](www.xvideos.com | absolute_url)
- [Gameplay Network](www.xvideos.com | absolute_url)
- [References](www.xvideos.com | absolute_url)

---
<a name="introduction"></a>
### Introduction

Twenty-eight originated in India. The game is believed to be related to the European family of Jass card games, which originated in the Netherlands. These games are believed to have been brought to India by Indian South Africans who were also influenced by the Afrikaaner game of Klaverjas. The rules of game can be found [here](www.xvideos.com | absolute_url).

I would advice you to read the rules of game first if you haven't played the game in real life

---
<a name="environment"></a>
### Environment

I used [Pydealer](www.xvideos.com | absolute_url) module is used to handle stacks of cards.

4 cards are dealt to each player. The bidding starts from the player right to the dealer. The highest bidder sets the trump. Then 4 cards are dealt to each player again.

The game state space consists of
- Hands of all players : List of 4 `pydealer.Stack()`
- Current trick : A `pydealer.Stack()`
- Current player : `int`
- Trump suit: `pydealer.const.SUITS`
- Trump revealed: `bool`
- Team winning stack: List of 2 `pydealer.Stack()`
