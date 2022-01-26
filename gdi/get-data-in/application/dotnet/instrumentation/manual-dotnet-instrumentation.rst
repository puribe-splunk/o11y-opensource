.. _dotnet-manual-instrumentation:

********************************************************************
Manually instrument .NET applications for Splunk Observability Cloud
********************************************************************

.. meta:: 
   :description: Manually instrument your .NET application to add custom attributes to spans or manually generate spans. Keep reading to learn how to manually instrument your .NET application for Observability Cloud. 

The SignalFx Instrumentation for .NET provides and registers an OpenTracing-compatible global tracer that you can use to instrument your applications manually for Splunk Observability Cloud. Custom or manual instrumentation can be helpful when you need to add custom attributes to spans, or need to generate spans manually.

.. note:: The SignalFx Instrumentation for .NET supports OpenTracing version 0.11.0 and higher.

To instrument your .NET application manually, follow these steps:

#. Add the OpenTracing dependency to your project:

   .. code-block:: xml

      <PackageReference Include="OpenTracing" Version="0.12.1" />

#. Obtain the ``OpenTracing.Util.GlobalTracer`` instance and create spans:

   .. code-block:: csharp

      using OpenTracing;
      using OpenTracing.Util;

      namespace MyProject
      {
         public class MyClass
         {
            public static async void MyMethod()
            {
               // Obtain the automatically registered OpenTracing.Util.GlobalTracer instance
               var tracer = GlobalTracer.Instance;

               // Create an active span that automatically becomes a child span of any existing span in this context
               using (IScope scope = tracer.BuildSpan("MyTracedFunctionality").StartActive(finishSpanOnDispose: true))
               {
                  var span = scope.Span;
                  span.SetTag("MyTag", "MyValue");
                  span.Log("My Log Statement");

                  var ret = await MyAppFunctionality();

                  span.SetTag("FunctionalityReturned", ret.ToString());
               }
            }
         }
      }