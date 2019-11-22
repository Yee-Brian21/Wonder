# Wonder
Wonder technical take home

## Question 1. Analysis

The average score of analysts is 3.9/5 for both sourcing and writing.
However, the median is a better indicator of overall analyst quality since the quartiles show that the distribution of scores is heavily left skewed. That means that most of the Analysts are highly rated.

Each analyst seems to have the same score for both sourcing and writing, can condense this to one rating or define clearer definitions of sourcing and writing.

Maximum wait time was 107 minutes, the minimum was 1 minute and the average is 5 minutes

There is around 2~3 analysts available at any given time to accept a job.

The number of free analysts peaked at 8.

Average of 15 Analysts working at once.

There are 71 Analysts and 74 Different Jobs available at this given time frame

Looking only at jobs that were accepted:
* 74 were sourcing
* 65 were review
* 54 were writing
* 35 were vetting
* 33 were planning
* 27 were source review
* 15 were editing

Can hire more or less analysts of certain roles to match this distribution

The longest time between job creation and job acceptance was 2 Days and 11 Hours while the shortest was less than 1 minute

## Question 2. Data Modeling

Github GIST : https://gist.github.com/Yee-Brian21/11fb1b778c6a0f524d56d282406c5c6b

I would create 3 tables, one for analysts, one for requests, and an intersection table between analysts and requests to keep track of what jobs are currently being worked on.

### Analyst Table:
* AnalystID (Primary Key) - unique ID to identify analysts.
* sourcingScore - the analysts's sourcing score.
* writingScore - the analysts's writing score.
* reviewsComp - the number of review jobs the analyst has completed.
* vettingComp - the number of vetting jobs the. analyst has completed.
* planningComp - the number of planning jobs the analyst has completed.
* editingComp - the number of editing jobs the analyst has completed.
* sourcingComp - the number of sourcing jobs the analyst has completed.
* writingComp - the number of writing jobs the analyst has completed.

### Requests Table:
* RequestID (Composite Key) - ID to identify client questions.
* jobType (Composite Key) - the type of the current job (Review, Vetting, Planning, etc.).
* postTime - the time the request was made.
* acceptedTime - the time when a request has been accepted by an analyst.
* completedTime - the time when a request has been completed by an analyst.

### CurrentJobs Table:
* AnalystID (Foreign Key)
* RequestID (Foreign Key)
* jobType (Foreign Key)


First, a client would create a request which will then be broken down into the different jobs that need to be completed.
These will all be entered into the Requests table as individual rows for each smaller task that needs to be completed. The current time of posting will also be initiated at this time.
When the job job gets accepted, the current time will be logged into `acceptedTime` and a row will be entered into the CurrentJobs. When the job is finally completed, the time will again be logged into `completedTime` and the corresponding row in the CurrentJobs table will be removed.

As for the Analyst Table, each analyst will have his/her own `uniqueID` as well as a `sourcingScore` and `writingScore`. I also want to keep track of how many jobs each analyst has completed and what kind of job it was. This will allow us to see what an analyst's strengths are as well as keep track of the analysts's productivity.

Since the requests table has time stamps for every step of the process, we can show how long it usually takes for requests to be fulfilled.

Allows us to group analyst by their rating, the number of jobs completed, types of jobs completed.

## Question 3. SQL

COALESCE only selects the first non-null value in the column. To get all non-null values, instead use the `IS NOT NULL` conditional.

Datetime comparison is also wrong. Need to compare to a properly formated string. String should instead be `2009-01-01`.
