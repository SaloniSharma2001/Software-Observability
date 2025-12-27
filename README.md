# Software-Observability

**What are Logs?**

<h2>Logs</h2>

<p>
Logs contain <strong>plain text or JSON</strong> to record events that occur during the processing of an
<strong>API request</strong> within an application.
</p>

<h3>Why Logs Are Important</h3>

<p>Logs can record the following levels:</p>

<ul>
  <li><strong>Information</strong></li>
  <li><strong>Warning</strong></li>
  <li><strong>Error</strong></li>
</ul>

<p>These are crucial for:</p>

<ul>
  <li>
    <strong>Debugging</strong> — understanding what happened within the application while processing an API request.
  </li>
  <li>
    <strong>Tracking errors and failures</strong> along with stack traces.
  </li>
  <li>
    <strong>Monitoring application behavior</strong> to detect unusual patterns.
  </li>
</ul>

<h3>API-Level Details Captured in Logs</h3>

<ul>
  <li>
    <strong>Incoming request details</strong>
    <ul>
      <li>URL</li>
      <li>HTTP method</li>
      <li>Headers</li>
      <li>Request body</li>
    </ul>
  </li>
  <li>
    <strong>Events occurring during API request processing</strong>
  </li>
  <li>
    <strong>Outgoing response details</strong>
    <ul>
      <li>Status code</li>
      <li>Time taken</li>
      <li>Response payload</li>
    </ul>
  </li>
</ul>

<h2>Limitation</h2>

<ul>
  <li>
    <strong>Logs</strong> do not show the <strong>API request end-to-end journey</strong> across microservices.
  </li>
  <li>
    We don’t know the <strong>time breakdown per service</strong>, which helps us understand where the request spent more time.
  </li>
</ul>

<p>
If a request goes through:
<strong>OrderService → PaymentService</strong>
</p>

<p>
Each application maintains its logs <strong>independently</strong>.
</p>

<pre>
[OrderService] Order created for userId=55
</pre>

<p>Similarly:</p>

<pre>
[PaymentService] Payment timeout userId=55
</pre>

<p>
But we can’t easily <strong>tie these logs together</strong> (for example, whether both logs belong to the same API request)
or determine <strong>how long each step took</strong>.
</p>

<img width="1378" height="670" alt="image" src="https://github.com/user-attachments/assets/46c03387-485c-44c1-b118-4fdf4dd7ac91" />

<h2>Tracing</h2>

<ul>
  <li>
    <strong>Tracing</strong> tracks the path of a request across <strong>microservices</strong>.
  </li>
</ul>

<p><strong>It shows high-level information such as:</strong></p>

<ul>
  <li>Which service called which service</li>
  <li>How much time each service took</li>
  <li>Status code of each service (200, 400, 500, etc.)</li>
</ul>

<p>
<strong>But tracing does NOT store detailed logs.</strong>
</p>

<h2>How Distributed Tracing Works</h2>

<p>
To achieve this, <strong>Distributed Tracing</strong> creates
<strong>TRACE_ID</strong> and <strong>SPAN_ID</strong>.
</p>

<ul>
  <li>
    <strong>Trace ID</strong>:
    Connects a request across multiple microservices using
    <strong>one unique identifier</strong>.
  </li>
</ul>

<ul>
  <li>
    <strong>Span ID</strong>:
    Each service has its own unique <strong>Span ID</strong>, which represents
    individual units of work performed inside that service.
  </li>
</ul>

<img width="1577" height="676" alt="image" src="https://github.com/user-attachments/assets/bf79c3e4-47ca-4cb2-b507-b124c55b008f" />

<p>We can make our own custom span and inside that a child span, just to know when the a particular operation started and when it ended. A child span will be part of parent span means task inside a task and how much time it took for that child task to execute will be observed with the help of child span.</p>

 <strong>When do we need to create a new span</strong>:

<ul>
  <li>When we have <strong>an incoming request</strong> to an application.</li>
    <li>When we create <strong>a new thread or thread handof</strong>.</li>
      <li>Within an application (When having atleast two application) <strong>trying to invoke a second application</strong> so for a duration from when the request initiated for invoking the second application till the response is returned</li>
</ul>



