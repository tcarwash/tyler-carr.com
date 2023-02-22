title: PylogCQ
date: 2021-10-2
category: Software
authors: Tyler Carr
keywords: linux, python, Amateur Radio, Radio Amateur, Ham Radio, Hamlib, Logging, Radioamador, Amateurfunk
tags: Software, Amateur Radio
status: published
summary: A curses based TUI logging program for amateur radio use. 

## What is PylogCQ, and why?
[PylogCQ](https://github.com/tcarwash/pylogcq) is a curses based TUI logging program for Amateur Radio use. 

![PylogCQ](https://content.ag7su.com/file/ag7su-web/pcq1.png)

I started writing PylogCQ because I wanted a dead simple logging program that I could use from the command line, and remotely. PylogCQ uses the `npyscreen` library to build it's UI and `Hamlib` for CAT control. 

## Behavior

PylogCQ is meant to be simple. 

PylogCQ exposes the `cq` endpoint when installed.

If no rig control arguments are passed to `cq` the TUI offers basic contact (QSO) information fields to the user for input. 
Frequency and Mode input is retained between QSOs, PylogCQ assumes you haven't changed these things unless you tell it you have. PylogCQ will also assume a perfect 599 signal report both ways if nothing is input. QSO time/date are recorded at the time of logging. These all combine to result in extremely fast logging of QSOs, leaving the user free to work the radio and make contacts. 

If a hamlib `rigctld` server is specified, as: `cq -r 127.0.0.1 -p 4534` PylogCQ reads the frequency and mode from a connected radio. 

PylogCQ writes each QSO to a running swap (recovery) file in JSON format, and converts that file to an ADIF on clean exit.

## Installation
PylogCQ is available on the Python Package Index (PyPI)

simply:

    :::bash
    python3 -m pip install pylogcq


