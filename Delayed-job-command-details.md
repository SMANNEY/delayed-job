Here is the command line detailed help :

    Usage: script/delayed_job [options] start|stop|restart|run
    -h, --help                       Show this message
    -e, --environment=NAME           Specifies the environment to run this delayed jobs under (test/development/production).
        --min-priority N             Minimum priority of jobs to run.
        --max-priority N             Maximum priority of jobs to run.
    -n, --number_of_workers=workers  Number of unique workers to spawn
        --pid-dir=DIR                Specifies an alternate directory in which to store the process ids.
    -i, --identifier=n               A numeric identifier for the worker.
    -m, --monitor                    Start monitor process.
        --sleep-delay N              Amount of time to sleep when no jobs are found
        --read-ahead N               Number of jobs from the queue to consider
    -p, --prefix NAME                String to be prefixed to worker process names
        --queues=queues              Specify which queue DJ must look up for jobs
        --queue=queue                Specify which queue DJ must look up for jobs
        --exit-on-complete           Exit when no more jobs are available to run. This will exit if all jobs are scheduled to run in the future.

Example :

    # Runs two workers in separate processes.
    RAILS_ENV=production script/delayed_job -n 2 start
    RAILS_ENV=production script/delayed_job stop

