### 基础理论
 **Distributed Tracing System components**
 - instrumentation library  
 One API and SDK per language.  
 Previously, Library did all the recording, collecting, sampling and aggregation on spans/stats/metrics, and exported them to other persistent storage backends via the Library exporters, or displayed them on local zpages.   
 - agent  
 An agent running with the application or on the same host as the application.   
  Agent runs as a daemon in the VM/container and can be deployed independent of Library. Once Agent is deployed and running, it should be able to retrieve spans/stats/metrics from Library, export them to other backends.  
 - collector  
 Optionally   
 The collector provides a central egress point for exporting traces and metrics to one or more tracing and/or metrics backends while offering buffering and retries as well as advanced aggregation, filtering, annotation and intelligent sampling capabilities.  
 A collector running as a standalone service.  
 - backend  
 jaeger、prometheus、zipkin

**sampling types**
- Head-based sampling (standard distributed tracing)   
 the sampling decision is made at the beginning of the request (i.e. when the root span is to be created). Head-based sampling is the most common form of sampling leveraged because it is easy to implement. The problem is that this sampling type makes the sampling decision at the beginning of the request so it is unaware of what might happen later in the request. This means head-based sampling is good at reducing verbosity but bad at ensuring relevant traces are kept.  
 又可细分为 固定比例采样(Probabilistic Sampling)、蓄水池采样(Rate Limiting Sampling)、混合采样(Adaptive Sample)。
- Tail-based sampling\Intelligent Sampling (Infinite Tracing)   
the sampling decision is made at the end of the request (i.e. when all spans for a given trace ID have been received).    
As a result, it is possible to capture relevant traces while minimally or never sampling less relevant traces.     
While tail-based sampling is often more desirable than head-based sampling, it does introduce additional complexity. For example, all spans for a given trace need to be processed by the same system and the trace cannot be ingested by a backend until the entire trace is collected and a sampling decision is made (it is hard to know when a trace is complete).   
In addition, this analysis requires more compute power and will need to be factored into how you architect your collection infrastructure.

**采集的数据**  
 traces（ a collection of spans）、metrics、 logs


### 业界解决方案
**历史**
- 2010年 Lightstep Co-Founder and CEO Ben Sigelman develops Dapper, one of the first large-scale distributed tracing infrastructures.  
- 2010年 Rob Benson, Lightstep's VP of Engineering, Product, & Design, is one of three engineers at Twitter helping to create Zipkin.  
- 2012年 Zipkin, which started as a hack day project shortly after the publication of the Dapper paper, is open-sourced by Twitter.  
- 2015年 Lightstep is founded by Ben Sigelman, Daniel Spoonhower, and Ben Cronin   
- 2016年 Lightstep helps to found OpenTracing, the first common, portable API for distributed tracing.
- 2017年 Jaeger, an open-source tracing tool developed by Yuri Shkuro, is open-sourced by Uber.   
- 2018年 Google releases a new portable instrumentation project called OpenCensus.  
- 2019年 The OpenTracing and OpenCensus projects announce a merger, forming OpenTelemetry
- 2020年 OpenTelemetry is released in Beta

**Distributed Tracing Systems**
- Google Dapper：不开源，此类系统的最早实现，很多类似系统受其启发
- 淘宝 EagleEye：不开源，2013年上线使用。
- 新浪 Watchman
- 京东 Hydra：开源，基本无人维护
- 大众点评 CAT：开源，基本无人维护
- Twitter zipkin ： 开源，受Google Dapper启发
- Uber Jaeger：开源，已经托管到 Cloud Native Computing Foundation (CNCF) 成为其第七个顶级项目， receives tracing telemetry data and provides processing, aggregation, data mining, and visualizations of that data. 受Google Dapper、zipkin 启发。
- OpenTracing：开源
- OpenCensus : 开源, from Google’s internal Census libraries ,在 Collector component中实现Tail-based sampling  
- OpenTelemetry（OpenTracing与OpenCensus合并）：开源，托管在 CNCF，provide APIs and SDKs in multiple languages to allow applications to export various telemetry data out of the process. 向后兼容OpenTracing、OpenCensus。
- Lightstep
- DataDog

### 资料
- [《Dapper, a Large-Scale Distributed Systems Tracing Infrastructure|Google Technical Report dapper-2010-1, April 2010》 ](https://static.googleusercontent.com/media/research.google.com/zh-CN//archive/papers/dapper-2010-1.pdf)
- [new relic](https://docs.newrelic.com/docs/understand-dependencies/distributed-tracing/get-started/how-new-relic-distributed-tracing-works)
- [Intelligent Sampling With OpenCensus](https://omnition.io/blog/intelligent-sampling-with-opencensus/)
- [So,youwant to traceyourdistributedsystem?Keydesign insightsfromyears ofpracticalexperience](https://www.pdl.cmu.edu/ftp/SelfStar/CMU-PDL-14-102.pdf)
- [Principledwork÷ow-centrictracing of distributedsystems](https://www.rajasambasivan.com/wp-content/uploads/2017/07/sambasivan-socc16.pdf)
-  Chapter 3 of 《Mastering Distributed Tracing》
- [Google Dapper](https://research.google/pubs/pub36356/)
- [opentelemetry docs](https://opentelemetry.io/docs/)
- 《Distributed Tracing in Practice: Instrumenting, Analyzing, and Debugging Microservices》
