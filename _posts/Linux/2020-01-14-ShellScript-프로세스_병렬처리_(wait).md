---
layout: post
title: '[ShellScript] 프로세스 병렬 처리 (wait)'
category: Linux
tags: [linux, shell script, wait]
comments: true
---

~~~bash
#!/bin/bash

# run processes and store pids in array
for i in $n_procs; do
    ./procs[${i}] &
    pids[${i}]=$!
done

# wait for all pids
for pid in ${pids[*]}; do
    wait $pid
done
~~~