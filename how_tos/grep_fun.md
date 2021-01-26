# grep fun
## grep multiple phrases with OR (match either OR)

    grep -E 'python2|python3'

## grep multiple phrases with AND (must match all phrases)

    ps aux  | grep root | grep PM | grep /System

(find all processes started in PM by root with a command starting with /System)

## find processes matching a name

    ps aux  | grep -E 'python2|python3'

## grep greps man page for the -i option

    man grep | grep '\-i

## filter a tailed log using grep on an ongoing basis

    tailf log | grep --line-buffered "some words" >> file
