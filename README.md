# sagemaker-experiment-run

The following components make up the building blocks of an experiment in Amazon SageMaker.

`experiment`: An experiment is a collection of runs. When you initialize a run in your training loop, you include the name of the experiment that the run belongs to. Experiment names must be unique within your AWS account.

`Run`: A run consists of all the inputs, parameters, configurations, and results for one interation of model training. Initialize an experiment run for tracking a training job with Run.init().

`load_run`: To run your experiments in script mode, refer to an initialized Run object with load_run(). If an experiment for a run exists, load_run returns the experiment context. Generally, you use load_run with no arguments to track metrics, parameters, and artifacts within a SageMaker training or processing job script.

Load run from a local script passing experiment and run names
```python
with load_run(experiment_name=experiment_name, run_name=run_name) as run:
   run.log_parameter("param1", "value1")
```
```python
# Load run within a training or processing Job (automated context sharing)
with load_run() as run:
    run.log_parameter("param1", "value1") 
```

`log_parameter`: Log parameters for a run, such as batch size or epochs, over time in a training loop with run.log_parameter(). log_parameter records a single name-value pair in a run. You can use run.log_parameters() to log multiple parameters. If called multiple times within a run for a parameter of the same name, log_parameter overwrites any previous value. The name must be a string and the value must be either a string, integer, or float.

```python
# Log a single parameter
run.log_parameter("param1", "value1")

# Log multiple parameters
run.log_parameters({
    "param2": "value2",
    "param3": "value3"
})
```
`log_metric`: Log metrics for a run, such as accuracy or loss, over time in a training loop with run.log_metric(). log_metric records a name-value pair where the name is a string and the value is an integer or float. To declare the frequency of logging over the course of the run, define a step value. You can then visualize these metrics in the Studio Experiments UI. For more information, see View, search, and compare experiment runs.

```python
# Log a metric over the course of a run
run.log_metric(name="Final_loss", value=finalloss)

# Log a metric over the course of a run at each epoch
run.log_metric(name="test:loss", value=loss, step=epoch)
```

`log_artifact`: Log any input or output artifacts related to a run with run.log_artifact(). Log artifacts such as S3 URIs, datasets, models, and more for your experiment to help you keep track of artifacts across multiple runs. is_output is True by default. To record the artifact as an input artifact instead of an output artifact, set is_output to False.

```python
# Track a string value as an input or output artifact
run.log_artifact(name="training_data", value="data.csv" is_output=False)
```

`log_file`: Log any input or output files related to a run, such as training or test data, and store them in Amazon S3 with run.log_file(). is_output is True by default. To record the file as an input artifact instead of an output artifact, set is_output to False.

```python
# Upload a local file to S3 and track it as an input or output artifact
run.log_file("training_data.csv", name="training_data", is_output=False)
```