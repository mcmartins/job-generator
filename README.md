# Broccoli

<img src="https://github.com/mcmartins/parallel-jobs/blob/master/docs/broccoli.png" alt="logo" width="200px" height="200px">

# README

The tool should be able to start a Job. The Job is constituted by a set of Tasks.
These Tasks can have Guidance Tasks (Sub Tasks) and so on, forming a kind of Tree.
Top Level Tasks will run in parallel and Guidance Tasks will follow as soon parent Tasks finishes.
The processing finishes when one top level Task finishes executing (including, if any, all its Guidance Tasks).

# API

The tool should accept as input a JSON String/File, parse it and start the processing.

The input JSON includes the following information:

* A Job to execute:
 1. Name
 2. Working directory;
 3. Timeout;
 4. Tasks

* For each Task:
 1. Name
 2. Command
 3. Wait
 4. Sub Tasks (Guidance)

## Example Input

The input will be something like:

```json
{
  "jobName": "Boolean Algebra 2-Basis",
  "workingDir": "/tmp/",
  "timeout": 60,
  "tasks": [
    {
      "taskName": "Get guiding interpretation",
      "wait": false,
      "command": "mace4 -n6 -m -1 -f BA2-interp.in | get_interps | isofilter ignore_constants wrap > BA2-interp.out",
      "guidance": [
        {
          "taskName": "Job with guidance (20 seconds)",
          "wait": false,
          "command": "prover9 -f BA2.in BA2-interp.out > BA2.out"
        }
      ]
    },
    {
      "taskName": "Job without guidance (46 seconds)",
      "wait": false,
      "command": "prover9 -f BA2.in > BA2-base.out"
    }
  ]
}
```

## Integration with WebGAP

A node.js module should be produced in order to wrap the python tool. This tool will be used as an asynchronous job executor.
The tool will be invoked over the WebGAP API and will run inside docker containers.

# Diagrams

Application Flow Diagram:

![alt text](https://github.com/mcmartins/parallel-jobs/blob/master/docs/flow.png)

Job Diagram

![alt text](https://github.com/mcmartins/parallel-jobs/blob/master/docs/job.png)

# Test

The following Job is being used as TestCase:

![alt text](https://github.com/mcmartins/parallel-jobs/blob/master/docs/test_job.png)

## MISSING TESTS IMPLEMENTATION

# LICENSE

Apache License, Version 2.0
