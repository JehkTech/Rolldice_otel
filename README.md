# RollDice

To start your Phoenix server:

  * Run `mix setup` to install and setup dependencies
  * Start Phoenix endpoint with `mix phx.server` or inside IEx with `iex -S mix phx.server`

Now you can visit [`localhost:4000`](http://localhost:4000) from your browser.

Ready to run in production? Please [check our deployment guides](https://hexdocs.pm/phoenix/deployment.html).

## Learn more

  * Official website: https://www.phoenixframework.org/
  * Guides: https://hexdocs.pm/phoenix/overview.html
  * Docs: https://hexdocs.pm/phoenix
  * Forum: https://elixirforum.com/c/phoenix-forum
  * Source: https://github.com/phoenixframework/phoenix


PROCESS -->

First, they have Logger configured with different backends for info, error, and warn logs. They're using several OpenTelemetry dependencies: opentelemetry, opentelemetry_api, exporter, phoenix, cowboy, and ecto. Their goal is to use these to read logs and show them in a UI.

Looking at their controller code, they’re using Logger to log messages and a custom Tracer module with spans. But OpenTelemetry isn't just about logging; it's for distributed tracing. So maybe the user is mixing logging with tracing. They might need to clarify that OpenTelemetry handles traces and metrics, not just logs. However, the dependencies listed include the exporter, which is for sending traces to a backend like Jaeger or Prometheus.

Wait, the user mentioned wanting to "read the logs" and display a UI. But OpenTelemetry's main role is for traces. The Logger backends they have are writing to files. So perhaps they want to export traces (not just logs) to a visualization tool. The UI part might involve setting up something like Jaeger or Zipkin to view the traces.

First, they need to configure the OpenTelemetry SDK. The dependencies are there, but they need to set up the OTel pipeline. That would involve initializing the SDK, adding exporters, and setting up processors.

The user's code uses a Tracer module. They should replace that with the OpenTelemetry API. The current code uses Tracer.with_span, which might be their own module. They need to switch to using :otel_tracer.start_span and set attributes properly.

Also, the OpenTelemetry Phoenix and Cowboy integrations will automatically capture HTTP requests. The Ecto integration will track database queries. But since this is a dice roller, maybe Ecto isn't used, but the setup is there.

For the UI part, they need an OpenTelemetry collector to receive the traces and export them to a backend like Jaeger. Then, Jaeger's UI can display the traces. Alternatively, they could use the OpenTelemetry exporter to send traces directly to Jaeger.


So, steps needed:

1. Configure the OpenTelemetry pipeline to export traces.

2. Replace the custom Tracer with OpenTelemetry's API.

3. Set up the Phoenix and Cowboy integrations to auto-instrument web requests.

4. Deploy a Jaeger instance to collect and visualize traces.

5. Update the controller to use OpenTelemetry spans correctly.

Wait, the user might not realize that logs and traces are different. Their Logger config is separate from OpenTelemetry. So they need to clarify that OpenTelemetry handles traces, which are different from application logs. However, OpenTelemetry can also handle logs, but that's a newer feature. The dependencies they have are more for tracing.

