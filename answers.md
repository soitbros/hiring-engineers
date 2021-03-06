Your answers to the questions go here.

# Collecting Metrics:
* Add tags in the Agent config file and show us a screenshot of your host and its tags on the Host Map page in Datadog.
![](https://i.imgur.com/SdtG1M4.png)
![](https://i.imgur.com/WwVjcGy.png)
* Install a database on your machine (MongoDB, MySQL, or PostgreSQL) and then install the respective Datadog integration for that database.
![](https://i.imgur.com/Mr3QTwv.png)
* Create a custom Agent check that submits a metric named my_metric with a random value between 0 and 1000.
![](https://i.imgur.com/FuT98iJ.png)
***Mostly I borrowed from the example. Why mess with perfection?***
* Change your check's collection interval so that it only submits the metric once every 45 seconds.
![](https://i.imgur.com/wt84oaH.png)

Bonus Question Can you change the collection interval without modifying the Python check file you created?
***Yep! Just change the metric.yaml file instead.***

# Visualizing Data:

Utilize the Datadog API to create a Timeboard that contains:

* Your custom metric scoped over your host.
* Any metric from the Integration on your Database with the anomaly function applied.
* Your custom metric with the rollup function applied to sum up all the points for the past hour into one bucket

Please be sure, when submitting your hiring challenge, to include the script that you've used to create this Timeboard.

`avg(last_4h):anomalies(avg:mysql.performance.user_time{*}, 'basic', 2, direction='both', alert_window='last_15m', interval=60, count_default_zero='true') >= 1`

`avg:my_metric{*}.rollup(avg, 60)`

Once this is created, access the Dashboard from your Dashboard List in the UI:

Set the Timeboard's timeframe to the past 5 minutes
***I couldn't figure out how to do this!***

Take a snapshot of this graph and use the @ notation to send it to yourself.
![](https://i.imgur.com/DGQCqsp.png)
***I wasn't able to send it to myself though :(***

Bonus Question: What is the Anomaly graph displaying?
***Any deviation above or below an average baseline activity for user_time in MySQL***

# Monitoring Data:

Since you’ve already caught your test metric going above 800 once, you don’t want to have to continually watch this dashboard to be alerted when it goes above 800 again. So let’s make life easier by creating a monitor.

Create a new Metric Monitor that watches the average of your custom metric (my_metric) and will alert if it’s above the following values over the past 5 minutes:

* Warning threshold of 500
* Alerting threshold of 800
* And also ensure that it will notify you if there is No Data for this query over the past 10m.

Please configure the monitor’s message so that it will:

* Send you an email whenever the monitor triggers.
* Create different messages based on whether the monitor is in an Alert, Warning, or No Data state.
* Include the metric value that caused the monitor to trigger and host ip when the Monitor triggers an Alert state.
![](https://i.imgur.com/VdbKjds.png)

When this monitor sends you an email notification, take a screenshot of the email that it sends you.
![](https://i.imgur.com/qORwfBO.png)
![](https://i.imgur.com/CSy79cw.png)

Bonus Question: Since this monitor is going to alert pretty often, you don’t want to be alerted when you are out of the office. Set up two scheduled downtimes for this monitor:

* One that silences it from 7pm to 9am daily on M-F,
* And one that silences it all day on Sat-Sun.
* Make sure that your email is notified when you schedule the downtime and take a screenshot of that notification.
![](https://i.imgur.com/yWyL6Q1.png)
![](https://i.imgur.com/5D8NGCb.png)

# Collecting APM Data:

Given the following Flask app (or any Python/Ruby/Go app of your choice) instrument this using Datadog’s APM solution:

`from flask import Flask
import logging
import sys

main_logger = logging.getLogger()
main_logger.setLevel(logging.DEBUG)
c = logging.StreamHandler(sys.stdout)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
c.setFormatter(formatter)
main_logger.addHandler(c)

app = Flask(__name__)

@app.route('/')
def api_entry():
    return 'Entrypoint to the Application'

@app.route('/api/apm')
def apm_endpoint():
    return 'Getting APM Started'

@app.route('/api/trace')
def trace_endpoint():
    return 'Posting Traces'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port='5050')`

Note: Using both ddtrace-run and manually inserting the Middleware has been known to cause issues. Please only use one or the other.

Bonus Question: What is the difference between a Service and a Resource?
***According to the documentation, a service is a bunch of processes that do the same job while resources are actions OF the service. The easiest way to think of this metaphorically is a fast food employee would be the service, while their resources would be using the register, frying French fries, and cleaning floors.***

Provide a link and a screenshot of a Dashboard with both APM and Infrastructure Metrics.
***https://app.datadoghq.com/dash/1025052/ 
![](https://i.imgur.com/zyRpvbW.png)
![](https://i.imgur.com/bbRi6pl.png)***

Please include your fully instrumented app in your submission, as well.
***I just used the Flask app provided :)***

Final Question:

Datadog has been used in a lot of creative ways in the past. We’ve written some blog posts about using Datadog to monitor the NYC Subway System, Pokemon Go, and even office restroom availability!

Is there anything creative you would use Datadog for?
***I think it would be neat to see what you could do with Datadog and the Hue API. I was an early adopter of Hue lights, which offer millions of colors in wifi-connected LED lightbulbs. I could set up a custom monitor to turn one of my lights red if my server goes down, for example.***
