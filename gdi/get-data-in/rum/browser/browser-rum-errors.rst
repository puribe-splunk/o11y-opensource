.. _browser-rum-data-js-errors:

********************************************************
Errors collected by the Splunk Browser RUM agent
********************************************************

.. meta::
   :description: Understand which errors are collected by the Browser RUM agent for Splunk Real User Monitoring (RUM).

The Browser RUM agent collects errors as spans with a duration of zero. Error spans carry a timestamp based on the time when the error occurred.

By default, the instrumentations collect errors from the following sources:

-  Uncaught/unhandled errors from the ``"error"`` event listener on ``window``
-  Unhandled promise rejections from the ``unhandledrejection`` event listener on ``window``
-  Error events from failing to load resources from the ``"error"`` event listener on ``document``
-  ``console.error`` errors logged to the console
-  ``SplunkRum.error`` errors which can't be logged but are still collected by the agent

Uncaught or unhandled errors
=============================================

The Browser RUM agent registers each uncaught or unhandled error as a span with name ``onerror``. Uncaught or unhandled errors can occur in following situations:

-  Errors that aren't caught by ``try {}`` and ``catch {}`` blocks
-  Errors thrown again in a ``catch`` block but not caught again
-  Syntax errors in files

The following examples show how the Browser RUM agent collects uncaught or unhandled errors:

.. tabs::

   .. tab:: Syntax error example

      Consider the following syntax error:

      .. code-block:: javascript

         var abc=;

      The Browser RUM agent collects the error using the following attributes:

      - ``component``: ``"error"``
      - ``"error"``: ``true``
      - ``error.message``: ``Unexpected token ';'``
      - ``error.object``: ``SyntaxError``
      - ``error.stack``: ``SyntaxError: Unexpected token ';'``

      .. note:: ``error.message`` and ``error.stack`` messages are browser-specific.

   .. tab:: Null reference example

      Consider the following ``null`` reference error:

      .. code-block:: javascript

         var test = null;
         test.prop1 = true;

      The Browser RUM agent collects the error using the following attributes:

      - ``component``: ``"error"``
      - ``"error"``: ``true``
      - ``error.message``: ``Cannot set property 'prop1' of null``
      - ``error.object``: ``TypeError``
      - ``error.stack``: ``TypeError: Cannot set property 'prop1' of null at...``

      .. note:: ``error.message`` and ``error.stack`` messages are browser-specific.

Uncaught promise rejections
=============================================

The Browser RUM agent registers each uncaught promise rejection as a span with name ``unhandledrejection``. Uncaught promise rejections can happen in the following situations:

-  Promise chain without a final ``.catch``
-  Error in promise chain, including re-throwing in ``catch``, without any subsequent ``catch`` block
-  A ``throw`` in an ``async`` function

The following examples show how the Browser RUM agent collects uncaught promise rejections:

.. tabs::

   .. tab:: Example 1

      Consider the following code:

      .. code-block:: javascript

         new Promise((resolve, reject) => {
            reject(new Error('broken'))
         })

      The Browser RUM agent collects the error using the following attributes:

      - ``component``: ``"error"``
      - ``"error"``: ``true``
      - ``error.message``: ``broken``
      - ``error.object``: ``"error"``
      - ``error.stack``: ``Error: broken at...``

   .. tab:: Example 2

      Consider the following code:

      .. code-block:: javascript

         new Promise((resolve, reject) => {
            resolve(null)
         }).then((val) => {
            val.prop = 1
         })

      The Browser RUM agent collects the error using the following attributes:

      - ``component``: ``"error"``
      - ``"error"``: ``true``
      - ``error.message``: ``Cannot set property 'prop' of null``
      - ``error.object``: ``TypeError``
      - ``error.stack``: ``TypeError: Cannot set property 'prop' of null at...``

      .. note:: ``error.message`` and ``error.stack`` messages are browser-specific.

Failing to load resources
=============================================

The Browser RUM agent registers each failure to load resources as a span with name ``eventListener.error``. Failing to load resources can happen when the server returns 4xx or 5xx status coded when loading images or scripts.

Consider the following example:

.. code-block:: html

   <!DOCTYPE html>
   <html>
   <head>
      [...]
   </head>
   <body>
      <img src="/missing-image.png" />
   </body>
   </html>

The Browser RUM agent collects the error using the following attributes:

- ``component``: ``"error"``
- ``"error"``: ``true``
- ``error.message``: ``"IMG"``
- ``error.object``: ``"https://example.com/missing-image.png"``
- ``error.stack``: ``""//html/body/img""``

.. note:: ``error.message`` and ``error.stack`` messages are browser-specific.

Console errors
=============================================

The Browser RUM agent registers each error logged using the console as a span with name ``console.error``. Console errors are a standard way in browsers to show messages in the developer console. The Browser RUM agent collects
console errors from ``try...catch`` blocks where you don't want or can't throw errors further in the stack.

The following examples show how the Browser RUM agent collects console errors:

.. tabs::

   .. tab:: Setting field value to null

      Consider the following code:

      .. code-block:: javascript

         try {
            someNull.anyField = 'value';
         } catch(e) {
            console.error('failed to update', e);
         }

      The Browser RUM agent collects the error using the following attributes:

      - ``component``: ``"error"``
      - ``"error"``: ``true``
      - ``error.message``: ``failed to update TypeError: Cannot set property 'anyField' of null``
      - ``error.object``: ``String``
      - ``error.stack``: ``"TypeError: Cannot set property 'anyField' of null at...``

      .. note:: ``error.message`` and ``error.stack`` messages are browser-specific.

   .. tab:: Error 404

      Consider the following code:

      .. code-block:: javascript

         axios.get('/users').then(users => {
            showUsers(users)
         }).catch(error => {
            showErrorMessage()
            console.error('error getting users', error)
         })

      The Browser RUM agent collects the error using the following attributes:

      - ``component``: ``"error"``
      - ``"error"``: ``true``
      - ``error.message``: ``"error getting users Error: Request failed with status code 404"``
      - ``error.object``: ``"String"``
      - ``error.stack``: ``"Error: Request failed with status code 404 [...] at XMLHttpRequest.l.onreadystatechange  axios.min.js:2:8373)"``

      .. note:: ``error.message`` and ``error.stack`` messages are browser-specific.

Splunk RUM errors
=============================================

The Browser RUM agent registers each error logged by invoking ``SplunkRum.error`` as a span with name: ``SplunkRum.error``. Using ``SplunkRum.error`` doesn't log an error in developer console of the browser: errors are sent along with other RUM telemetry and exposed in the Splunk RUM UI. 

Consider the following example:

.. code-block:: javascript

   axios.get('/users').then(users => {
      showUsers(users)
   }).catch(error => {
      showErrorMessage()
      if (window.SplunkRum) {
         SplunkRum.error('error getting users', error)
      }
   })

The resulting error has similar attributes to any ``console.error`` collected by the Browser RUM agent.

Integrating with single-page application frameworks
========================================================

To enable the collection of JavaScript errors from single-page application (SPA) frameworks using their own error interceptors or handlers, you need to integrate the Browser RUM agent with the framework.

The following framework-specific examples show how to integrate with framework supported by Splunk RUM for Browser. All the examples assume that the Browser RUM agent is installed using npm. See :ref:`rum-browser-install-npm`.

.. tabs::

   .. tab:: React

      Use the Splunk RUM agent API in your error boundary component:

      .. code-block:: javascript

         import React from 'react';
         import SplunkRum from '@splunk/otel-js-browser';
         
         class ErrorBoundary extends React.Component {
            componentDidCatch(error, errorInfo) {
               SplunkRum.error(error, errorInfo)
            }
         
            // Rest of your error boundary component
            render() {
               return this.props.children
            }
         }

   .. tab:: Vue.js

      Add the collect function to your Vue ``errorHandler``. 
      
      For Vue.js version 3.x, use the following:

      .. code-block:: javascript

         import Vue from 'vue';
         import SplunkRum from '@splunk/otel-js-browser';
         
         const app = createApp(App);
         
         app.config.errorHandler = function (error, vm, info) {
            SplunkRum.error(error, info)
         }
         app.mount('#app')

      For Vue.js version 2.x, use the following:

      .. code-block:: javascript

         import Vue from 'vue';
         import SplunkRum from '@splunk/otel-js-browser';
         
         Vue.config.errorHandler = function (error, vm, info) {
            SplunkRum.error(error, info)
         }

   .. tab:: Angular

      For Angular version 2.x, create an error handler module:

      .. code-block::

         import {NgModule, ErrorHandler} from '@angular/core';
         import SplunkRum from '@splunk/otel-js-browser';
         
         class SplunkErrorHandler implements ErrorHandler {
            handleError(error) {
               SplunkRum.error(error, info)
            }
         }
         
         @NgModule({
            providers: [
               {
                  provide: ErrorHandler,
                  useClass: SplunkErrorHandler
               }
            ]
         })
         class AppModule {}

      For Angular version 1.x, create an ``exceptionHandler``:

      .. code-block:: javascript

         import SplunkRum from '@splunk/otel-js-browser';

         angular.module('...')
            .factory('$exceptionHandler', function () {
               return function (exception, cause) {
                  SplunkRum.error(exception, cause)
               }
         })

   .. tab:: Ember.js

      Configure an ``Ember.onerror`` hook:

      .. code-block:: javascript

         import Ember from 'ember';
         import SplunkRum from '@splunk/otel-js-browser';

         Ember.onerror = function(error) {
            SplunkRum.error(error)
         }

.. tip:: To avoid load issues due to content blockers when using the CDN version of the Browser RUM agent, add ``if (window.SplunkRum)`` checks around ``SplunkRum`` API calls. 