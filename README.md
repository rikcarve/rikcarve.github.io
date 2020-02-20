## 2020-02-20 Quarkus, Qute and Bootstrap
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

## 2017-02-03 Jenkins Docker Image
At some point everything *must* be dockerized, so I decided to dockerize our jenkins build server

## Welcome
I'll post stuff that might be useful for other's, or, at least for myself once it passed the store-in-brain expiry date ;-) 
