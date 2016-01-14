# ReliableSignalRMessaging

## Overview

A reliable SignalR server messaging framework prototype. This solution attempts to address the number of challenges when using SignalR as a server messaging framework.  It adds the following reliability features:

## Reliable messaging features

 * Robust management of client Hub connection including reconnection retry, managed disposal and guaranteed single connection at any point in time (which avoids duplicate message delivery).
 * To ensure Hub to Hub Client notifications can be retried, the notifications are sent from NServiceBus command handler via HubContext (rather than from within a Signalr Hub).
 * Hub Client immediately moves received messages from the volatile ring buffer to a persistent queue before processing. This avoids message loss if a Hub Client reconnects whilst messages are sitting in the buffer waiting to be processed.
 * Hub notification throttling on Hub server makes certain no notifications are pushed to a client whilst that Hub Client is reconnecting due to an access token refresh. This avoids message loss.
 * Hub Client message acknowledgments not sent back to the Hub. They are posted to a separately hosted Web Api.  This avoids lost acknowledgements where the Hub is temporarily unavailable.
 * On-client-connect replay policy that checks for unacknowledged dead letters (when a client connection is received) and replays them to the newly connected client. Messages are only replayed where they fall within a given date and number of retries and have not received an acknowledgment.
 * Hub client processing is managed on a separate thread in an Nservicebus command handler. This allows for multiple retries of insertion of data into the back office.
 * Hub Client message acknowledgment sending is managed on a separate thread in an NServicebus command handler.  This allows for multiple retries of insertion of data into the back office.
 * Future feature - Client side message store using Esent to ensure message Idempotency.

## Other features

* Secure SignalR connections using OAuth2 with JWT.
* Hosting of SignalR on Katana.
* Hosting of OAuth2 Token provider on Katana.
* Workflow managed via NServiceBus Saga.
* Signalr Hub Log4Net tracing.
* Full auditing of all messages sent and received

## To run solution

1. Create a SQL Server 2014 database called 'NServiceBus' on a SQL instance called '.\sqlserver2014'
2. Add an NserviceBus license to C:\NServiceBus\License.xml
3. Set the following projects to run in the solution: Audit, DocumentDownloader, Hub, HubSubscriber, IdentityProvider, OnlineBackOffice, Saga and Ui.
3. On the UI home page select the type of scenario and submit.

