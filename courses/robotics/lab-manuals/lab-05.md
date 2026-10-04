# Lab Manual 5 — ROS 2 Publisher & Subscriber

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Build a ROS 2 package with a custom publisher and subscriber node.

## Setup
Create a new ROS 2 package `lab05_pkg`.

## Procedure
1. **Task A — Publisher:** write a node that publishes a custom numeric message (e.g., a
   simulated sensor reading) on a topic at 2 Hz.
2. **Task B — Subscriber:** write a node that subscribes to that topic and logs each received
   value plus a running average.
3. **Task C — CLI verification:** with both nodes running, use `ros2 topic hz` and `ros2 topic
   echo` to confirm the publish rate and message content.
4. **Task D — Mini-challenge:** add a second subscriber node that only logs values above a
   threshold (passed in as a parameter).

## Expected Output
A working ROS 2 package with all four nodes/behaviors, plus a short report with CLI tool output.

## Submission
Submit the package + report by the end of the lab session.
