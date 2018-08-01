schedule
========


.. image:: https://api.travis-ci.org/dbader/schedule.svg?branch=master
        :target: https://travis-ci.org/dbader/schedule

.. image:: https://coveralls.io/repos/dbader/schedule/badge.svg?branch=master
        :target: https://coveralls.io/r/dbader/schedule

.. image:: https://img.shields.io/pypi/v/schedule.svg
        :target: https://pypi.python.org/pypi/schedule

Python job scheduling for humans.

An in-process scheduler for periodic jobs that uses the builder pattern
for configuration. Schedule lets you run Python functions (or any other
callable) periodically at pre-determined intervals using a simple,
human-friendly syntax.

Inspired by `Adam Wiggins' <https://github.com/adamwiggins>`_ article `"Rethinking Cron" <https://adam.herokuapp.com/past/2010/4/13/rethinking_cron/>`_ and the `clockwork <https://github.com/Rykian/clockwork>`_ Ruby module.

Features
--------
- A simple to use API for scheduling jobs.
- Very lightweight and no external dependencies.
- Excellent test coverage.
- Tested on Python 2.7, 3.5, and 3.6

Usage
-----

.. code-block:: bash

    $ pip install schedule

.. code-block:: python
    import os
    import subprocess
    import time
    import logging
    import datetime as dt
    from dateutil.relativedelta import relativedelta

    from schedule import Scheduler


    logger = logging.getLogger('set_running_jobs')
    logger.setLevel(logging.DEBUG)

    # create console handler and set level to debug
    ch = logging.StreamHandler()
    ch.setLevel(logging.DEBUG)

    # create formatter
    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

    # add formatter to ch
    ch.setFormatter(formatter)

    # add ch to logger
    logger.addHandler(ch)


    def job():
        res = subprocess.check_call('jupyter nbconvert --execute --to python --stdout "{}"'.format(FILE_TEST_PATH), shell=True)
        assert res == 0
        print("I'm working...")

    schedule = Scheduler()

    # Run each 5 weeks at 22:30
    # logger.info('Next run should be: {}'.format(now() + relativedelta(weeks=+5, hour=22, minute=30)))
    schedule.every(5).weeks.at('22:30').do(job)

    # Run 1st day of each month
    # logger.info('Next run should be: {}'.format(now() + relativedelta(months=+1, day=1, hour=22, minute=42, second=0)))
    schedule.every().first_of_month.at('22:42').do(job)

    # Run everyday at 14:00
    # logger.info('Next run should be: {}'.format(now() + relativedelta(days=+1, hour=14, minute=0)))
    schedule.every().day.at('14:00').do(job)

    # Run every Monday at 15:00
    schedule.every().monday.at('15:00').do(job)

    # Run each hour
    # logger.info('Next run should be: {}'.format(now() + relativedelta(hours=+1)))
    schedule.every().hour.do(job)

    # Run once the 31st, July 2018 at 17:22
    # logger.info('Next run should be: {}'.format(dt.datetime.strptime('2018-07-31 17:22', '%Y-%m-%d %H:%M')))
    schedule.once().at_date('2018-07-31 17:22').do(job)

    # More on the documentation: https://schedule.readthedocs.io/en/stable/faq.html#how-can-i-run-a-job-only-once
      
    # Run each minute
    # logger.info('Next run should be: {}'.format(now() + relativedelta(minutes=+1)))
    schedule.every().minute.do(job)

    # Run each 2 minutes
    # logger.info('Next run should be: {}'.format(now() + relativedelta(minutes=+2)))
    schedule.every(2).minutes.do(job)

    # Cancel previously registered jobs.
    schedule.clear

    while 1:
        # Run scheduled jobs
        schedule.run_pending()
        time.sleep(10)

Documentation
-------------

Schedule's documentation lives at `schedule.readthedocs.io <https://schedule.readthedocs.io/>`_.

Please also check the FAQ there with common questions.


Meta
----

Daniel Bader - `@dbader_org <https://twitter.com/dbader_org>`_ - mail@dbader.org

Distributed under the MIT license. See ``LICENSE.txt`` for more information.

https://github.com/dbader/schedule
