# SignalR_JS_Client_ (Demoweb app razor)

-first i will create razor web app,then add clientside library in js/signalr/ ; the library calees @microsoft/signalr.

-the Hub in signalr like the controller in tradintional web app so we create a folder Hub and create chatHub class and inherit from the hub class.

-then create a method sendMessage in chathub class .

-after that i will configure signalr in middleware in program class to add the signalservice and map the chatHubclass to the root /chathub.

-then we create your view in index.html file.

-after that i create chat.js file to configure the connection and eventListener from the client to the server.
<hr/>
<h2>information about HubClass:</h2>

-first of all we should know that signalr contains two important layer (connection Layer,Hub Layer); the connection layer faciliates for us the complexity of standing the web socket protocole or longPolling in other cases,hup layer is what we dealing with through hub class.

-<b>Hub class contains Context Property it is of type HubCallerContext and this object contains property and methods important for us:</b>
<h3>context Object properties</h3>
<table aria-label="Table 1" class="table table-sm margin-top-none">
<thead>
<tr>
<th>Property</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.connectionid" class="no-loc" data-linktype="absolute-path">ConnectionId</a></td>
<td>Gets the unique ID for the connection, assigned by SignalR. There's one connection ID for each connection.</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.useridentifier" class="no-loc" data-linktype="absolute-path">UserIdentifier</a></td>
<td>Gets the <a href="https://learn.microsoft.com/en-us/aspnet/core/signalr/groups?view=aspnetcore-8.0" data-linktype="relative-path">user identifier</a>. By default, SignalR uses the <a href="/en-us/dotnet/api/system.security.claims.claimtypes.nameidentifier#system-security-claims-claimtypes-nameidentifier" class="no-loc" data-linktype="absolute-path">ClaimTypes.NameIdentifier</a> from the <a href="/en-us/dotnet/api/system.security.claims.claimsprincipal" class="no-loc" data-linktype="absolute-path">ClaimsPrincipal</a> associated with the connection as the user identifier.</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.user" class="no-loc" data-linktype="absolute-path">User</a></td>
<td>Gets the <a href="/en-us/dotnet/api/system.security.claims.claimsprincipal" class="no-loc" data-linktype="absolute-path">ClaimsPrincipal</a> associated with the current user.</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.items" class="no-loc" data-linktype="absolute-path">Items</a></td>
<td>Gets a key/value collection that can be used to share data within the scope of this connection. Data can be stored in this collection and it will persist for the connection across different hub method invocations.</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.features" class="no-loc" data-linktype="absolute-path">Features</a></td>
<td>Gets the collection of features available on the connection. For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.connectionaborted" class="no-loc" data-linktype="absolute-path">ConnectionAborted</a></td>
<td>Gets a <a href="/en-us/dotnet/api/system.threading.cancellationtoken" class="no-loc" data-linktype="absolute-path">CancellationToken</a> that notifies when the connection is aborted.</td>
</tr>
</tbody>
</table>

<h3>context object Methods</h3>
<table aria-label="Table 2" class="table table-sm margin-top-none">
<thead>
<tr>
<th>Method</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.gethttpcontextextensions.gethttpcontext" class="no-loc" data-linktype="absolute-path">GetHttpContext</a></td>
<td>Returns the <a href="/en-us/dotnet/api/microsoft.aspnetcore.http.httpcontext" class="no-loc" data-linktype="absolute-path">HttpContext</a> for the connection, or <code>null</code> if the connection isn't associated with an HTTP request. For HTTP connections, use this method to get information such as HTTP headers and query strings.</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.abort" class="no-loc" data-linktype="absolute-path">Abort</a></td>
<td>Aborts the connection.</td>
</tr>
</tbody>
</table>
<hr/>

<b>-and it has also clients property which is of type IHubCallerClients and it has important properties and method:</b>

<h3>client Object Properties</h3>

<table aria-label="Table 3" class="table table-sm margin-top-none">
<thead>
<tr>
<th>Property</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all" class="no-loc" data-linktype="absolute-path">All</a></td>
<td>Calls a method on all connected clients</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubcallerclients-1.caller" class="no-loc" data-linktype="absolute-path">Caller</a></td>
<td>Calls a method on the client that invoked the hub method</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubcallerclients-1.others" class="no-loc" data-linktype="absolute-path">Others</a></td>
<td>Calls a method on all connected clients except the client that invoked the method</td>
</tr>
</tbody>
</table>

<h3>client Object Methods</h3>
<table aria-label="Table 4" class="table table-sm margin-top-none">
<thead>
<tr>
<th>Method</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.allexcept" class="no-loc" data-linktype="absolute-path">AllExcept</a></td>
<td>Calls a method on all connected clients except for the specified connections</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.client" class="no-loc" data-linktype="absolute-path">Client</a></td>
<td>Calls a method on a specific connected client</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.clients" class="no-loc" data-linktype="absolute-path">Clients</a></td>
<td>Calls a method on specific connected clients</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.group" class="no-loc" data-linktype="absolute-path">Group</a></td>
<td>Calls a method on all connections in the specified group</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.groupexcept" class="no-loc" data-linktype="absolute-path">GroupExcept</a></td>
<td>Calls a method on all connections in the specified group, except the specified connections</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.groups" class="no-loc" data-linktype="absolute-path">Groups</a></td>
<td>Calls a method on multiple groups of connections</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubcallerclients-1.othersingroup" class="no-loc" data-linktype="absolute-path">OthersInGroup</a></td>
<td>Calls a method on a group of connections, excluding the client that invoked the hub method</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.user" class="no-loc" data-linktype="absolute-path">User</a></td>
<td>Calls a method on all connections associated with a specific user</td>
</tr>
<tr>
<td><a href="/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.users" class="no-loc" data-linktype="absolute-path">Users</a></td>
<td>Calls a method on all connections associated with the specified users</td>
</tr>
</tbody>
</table>
</hr>
<h2>Notes about Hubs:</h2>

-an alternative for sendasync method is that we can strongly type the method because if the client miss or mespelled the name of the event we will get run time error so we create an interface have the methodname and takes the parameter then in hub class we inherit from Hub<interfaceHub> so we get compiletime error if there any thing wrong; then we can use this method instead of send async.
<hr/>
<h2>Refernces:</h2>

-https://learn.microsoft.com/en-us/aspnet/core/tutorials/signalr?view=aspnetcore-8.0&tabs=visual-studio

-https://learn.microsoft.com/en-us/aspnet/core/signalr/hubs?view=aspnetcore-8.0

-https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all?view=aspnetcore-8.0
