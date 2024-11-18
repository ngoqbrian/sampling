# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 1000 (from the original 50000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Brian Ngo

```
The model breaks sampling into the intital infection, primary contact tracing, and then secondary contact tracing.

For the inital infection,
    the function used was np.random.choice
    the sampling size was determined by the code:
        size=int(len(ppl) * ATTACK_RATE)
    the sampling frame was the list of all indices of all rows in the 'ppl' DataFrame
        which included 200 wedding attendees and 800 brunch atteendees for a total of 1000 individuals
    with the underlying distribution being uniform, where any individual had an equal chance of being selected

For the primary contact tracing,
 ppl.loc[ppl['infected'], 'traced'] = np.random.rand(sum(ppl['infected'])) < TRACE_SUCCESS
    the function used was np.random.choice
    Since TRACE_SUCCESS = 0.20, this meant that each infected individual had a independent probability of being traced by 20%
    Since ATTACK_RATE = 0.10 and len(ppl) = 1000, then the sample size is 100, representing the total number of individuals infected initally
    Sampling Frame would be rows in the the 'ppl' DataFrame where 'infected' is 'True'

For the secondary contact tracing,
    SECONDARY_TRACE_THRESHOLD = 2 means that secondary contact tracing occurs when 2 individuals were successfully traced in primary contact at a specific event. If the number of primacy contact traced infected individuals from an event meets the SECONDARY_TRACE_THRESHOLD, then all infected individuals at that event become traced during secondary tracing

The code does NOT appear to reproduce the graphs from the original blog post. 
After changing the simulation to 1000, and running the script multiple times, the outputted graphs varied each time. This would make it difficult to reproduce and identify subtle changes or trends. 

To make the model reproducible, I added to the code:
np.random.seed(42)
This will ensure that the output is the same everytime that the code is executed. Without a fixed random seed, the functions np.random.rand and np.random.choice would slightly vary each time.


```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `HH:MM AM/PM - DD/MM/YYYY`
* The branch name for your repo should be: `sampling-and-reproducibility`
* What to submit for this assignment:
    * This markdown file (sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `sampling-and-reproducibility`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-3-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
