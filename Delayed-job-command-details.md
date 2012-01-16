Here is the command line detailed help :

    Usage: script/delayed_jobs [options] start|stop|restart|run
     -h, --help : Show this message
        The -e/--environment option has been deprecated and has no effect.
        Use RAILS_ENV and see http://github.com/collectiveidea/delayed_job/issues/#issue/7
    --min-priority N : Minimum priority of jobs to run.
    --max-priority N : Maximum priority of jobs to run.
    -n, --number_of_workers=workers : Number of unique workers to spawn
    --pid-dir=DIR : Specifies an alternate directory in which to store the process ids.
    -i, --identifier=n : A numeric identifier for the worker.
    -m, --monitor : Start monitor process.
    --sleep-delay N : Amount of time to sleep when no jobs are found
    -p, --prefix NAME : String to be prefixed to worker process names

Example :

    # Runs two workers in separate processes.
    RAILS_ENV=production script/delayed_job -n 2 start
    RAILS_ENV=production script/delayed_job stop

(more examples to come)