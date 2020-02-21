---
layout: post
title: Qute Quarkus
tags: quarkus,qute
desc: Some impressions using Quarkus and Qute
---
# 2020-02-20 Quarkus, Qute and Bootstrap
I was never really into web UI, I have zero javascript knowledge and I'm kind of proud of that :-)
But at some point it is helpful if one can just create a simple little web gui and when I read about Quarkus and Qute I wanted to give it a try. Here are my impressions

```java
    @GET
    @Produces(MediaType.TEXT_HTML)
    public TemplateInstance get() {
        List<Light> data = new ArrayList<>();
        data.add(new Light("Kitchen", Switch.ON));
        data.add(new Light("Bedroom", Switch.OFF));
        return light.data("light", data);
    }
```
