---
title: "Liveness probe hack for Shiny apps"
layout: single
excerpt: ""
tags: [til, python]
---

Having spent all week creating a [shiny](https://shiny.posit.co/) new UI, I was very excited to deploy it to my company's [Kubernetes](https://kubernetes.io/docs/concepts/overview/) cluster. I was about to congratulate myself on a job well done, when my baby app was killed before my eyes.

What happened? The app worked all week when I ran my docker container locally. 

I checked the logs and saw several IP addresses trying to access the `/health` address on my app... and that is how I learnt about the [liveness probe](https://kubernetes.io/docs/concepts/configuration/liveness-readiness-startup-probes/).

In order to keep services running, Kubernetes will ping the `/health` endpoint of an app to check that it is still functioning. If there is no response, as in my case, then Kubernetes will kill the cluster and restart the app elsewhere.

The only problem is that the Shiny documentation doesn't describe an easy way to implement such an endpoint. In the end, I found that Shiny runs on top of the [starlette](https://www.starlette.io/) package for async web servers. This little hack was all it took to save my app

```python
from shiny import App
from starlette.responses import Response
from starlette.routing import Route

# code for the app_ui and server

#create the shiny app
app = App(app_ui, server)

#Define a health endpoint
async def health_check(request):
	return Response(status_code=200)

# need to insert as first element
app.starlette_app.router.routes.insert(0, Route("/health", health_check))
```
