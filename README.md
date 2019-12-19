# DynamiteJS

Dynamite (**DYNAMI**c **T**ask **E**ngine) lets you blast through the challenges of data-driven workflows.

## What is a data-driven workflow?

It's common for software, whether command-line program, HTTP API, or serverless function, to be a data-driven workflow at its core.
A data-driven workflow is a process that starts with some data and performs a series of steps based on that data.
Those steps may be to fetch additional info, do math, perform map/reduce operations on the data, or submit a record to another system.
**Dynamite** helps to declare the steps of a data-driven workflow, and to execute the workflow with any given input data.

## Wick

Wick (**W**orkflow **I**nstru**CK**tions) is the language that describes a workflow. Fine, it's not *really* a language, but a YAML or JSON data document=.
A named workflow is represented by a nested object where each key is a step of the workflow and each value is the step's instruction.
An instruction can be a literal value, or a [Jsonata](https://docs.jsonata.org/overview.html) expression.

    addNumbers:  # the name of the workflow
      $result: $1+$2  # one step which adds the two inputs

### Web Server

Use `dynamite-express` to allow workflows to be invoked via HTTP requests.

    "GET /month":  # the name of the workflow is now an HTTP binding expression
      $monthNum: $substring($now(),4,2)
      months: [Jan,Feb,Mar,Apr,May,Jun,Jul,Aug,Sep,Oct,Nov,Dec]
      $return: $months[$monthNum-1]

### Serverless

The `dynamite-aws` module knows how to interpret API Gateway requests,
so when handled by a Lambda function, the HTTP binding expression can also be used.

    "POST /cleanup/{taskName}":
      url: http://api.data.com/tasks/${taskName}
      $DELETE($url & '?olderThan=' & $toMillis($now()))

