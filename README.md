<div align="center">

## Build a Web Service that uses Authorization


</div>

### Description

In this article, we will develop a .NET Web Service in C# that requires authorization credentials.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Chris Harrison](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/chris-harrison.md)
**Level**          |Intermediate
**User Rating**    |4.6 (32 globes from 7 users)
**Compatibility**  |C\#, ASP\.NET
**Category**       |[Complete Applications](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/complete-applications__10-7.md)
**World**          |[\.Net \(C\#, VB\.net\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/net-c-vb-net.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/chris-harrison-build-a-web-service-that-uses-authorization__10-2252/archive/master.zip)





### Source Code


						<h4>Build a Web Service that uses Authorization</h4>
						<table border="0" width="450" cellspacing="0" cellpadding="0">
							<tr>
								<td width="100%">
									<p>In this article, we will develop a .NET Web Service in C# that requires
										authorization.</p>
									<p>The Web Service will expose the standard “Hello World” web method, and for the
										client to be able to receive the hello, it will need to send its credentials in
										the form of a username and password sent along with the SOAP call.</p>
									<p><STRONG>Lets build the Service first.</STRONG></p>
									<p>Using Visual Studio, I am going to create a new project called
										“HelloWorldAuthorized” that is a ASP.NET Web Service in C#.</p>
									<p>Now lets use the Service1.asmx class to implement our Web Service method. Here
										is the code we will add:</p>
									<p>The AuthHeader class is the class that will hold the authorization information,
										and should be public to allow the client that consumes this web service to
										access the class. This class is a child class of SoapHeader. The client will
										populate its fields and send it to the service with the public variable
										“Header.”</p>
									<blockquote><FONT face="Courier New"><FONT size="2"><FONT color="blue">public</FONT> <FONT color="blue">
													class</FONT> AuthHeader : SoapHeader
												<BR>
												{
												<BR>
												&nbsp;&nbsp;&nbsp; <FONT color="blue">public string</FONT> UserName;
												<BR>
												&nbsp;&nbsp;&nbsp; <FONT color="blue">public string</FONT> UserPassword;
												<BR>
												}
												<BR>
												<BR>
												<FONT color="blue">public</FONT> AuthHeader Header;</FONT></FONT> </blockquote>
									<p>The actual Web Service that does the work is the HelloWorld() method as
										described below.</p>
									<p>We will need to import the System.Web.Services.Protocal namespace into our
										class:</p>
									<blockquote><FONT face="Courier New"><FONT size="2"><FONT color="blue">using</FONT> System.Web.Services.Protocols;</FONT></FONT></blockquote>
									<p>Now we will code our Web Method:</p>
									<blockquote><FONT face="Courier New" size="2">[WebMethod]<BR>
											[SoapHeader("Header",Required=true)]
											<BR>
											<FONT color="blue">public string</FONT> HelloWorld()
											<BR>
											{&nbsp;<BR>
											&nbsp; <FONT color="blue">if</FONT> (Header.UserName.ToLower() == "harrison")<BR>
											&nbsp; {&nbsp;<BR>
											&nbsp;&nbsp;&nbsp;&nbsp; <FONT color="blue">return</FONT> "Hello World!";&nbsp;<BR>
											&nbsp; }&nbsp;<BR>
											&nbsp; <FONT color="blue">else</FONT>&nbsp;<BR>
											&nbsp;&nbsp;{&nbsp;<BR>
											&nbsp;&nbsp;&nbsp;&nbsp; <FONT color="blue">return</FONT> "you are not
											authorized";&nbsp;<BR>
											&nbsp; }<BR>
											} </FONT></blockquote>
									<p>We have out standard [WebMethod] attribute and the [SoapHeader] to specify that
										we expect the authorization information to be passed to this Web Method. In
										addition, we are going to check this authorization information that we receive.
										For this one, I am only checking the Username, not the password, and I am
										converting it all “ToLower()” to eliminate the case sensitivity. Of course, in
										the real world it would be best to add some additional security to this.</p>
									<p><STRONG>Now for the Client:</STRONG></p>
									<P>I am going to add a new Visual Studio Project to my solution called
										“HelloWorldClient” that is an ASP.NET Web Application, in C#.
									</P>
									<P>First, I will need to add a Web Reference to my Service I created above. To do
										that in Visual Studio, right-click the “HelloWorldClient” project and choose
										“add a Web Reference.” In the URL box, enter the URL to our Web Service,
										“http://localhost/HelloWorldAuthorized/Service1.asmx.” You should then see the
										service description and then click “Add Reference” to add it to our
										project.&nbsp; You&nbsp;should now see the reference show up as “localhost”
										under the Web References section of the Visual Studio Solution Explorer.
									</P>
									<P>Now for the code.
									</P>
									<P>For our implementation, I added a Label control onto the WebForm1.aspx page
										created in our HelloWorldClient project, called Label1. Then, in the Page_Load
										event, I added the call to the Web Service, as shown below:
									</P>
									<BLOCKQUOTE dir="ltr" style="MARGIN-RIGHT: 0px">
										<P><FONT size="2"><FONT face="Courier New"><FONT color="blue">private void</FONT> Page_Load(<FONT color="blue">object</FONT>
													sender, System.EventArgs e)
													<BR>
													{
													<BR>
													&nbsp;&nbsp;&nbsp; localhost.AuthHeader auth =
													<BR>
													&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
													<FONT color="blue">new </FONT>localhost.AuthHeader();<BR>
													&nbsp;&nbsp;&nbsp; auth.UserName = "Harrison";<BR>
													&nbsp;&nbsp;&nbsp; auth.UserPassword = "password";<BR>
													&nbsp;&nbsp;&nbsp;
													<BR>
													&nbsp;&nbsp;&nbsp; localhost.Service1 ws =
													<BR>
													&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
													<FONT color="blue">new</FONT> localhost.Service1();
													<BR>
													&nbsp;&nbsp;&nbsp; ws.AuthHeaderValue = auth;
													<BR>
													&nbsp;&nbsp;&nbsp;
													<BR>
													&nbsp;&nbsp;&nbsp; Label1.Text = ws.HelloWorld();
													<BR>
													}</FONT> </FONT>
										</P>
									</BLOCKQUOTE>
									<P>First, we create an instance of the AuthHeader class on the client, called
										“auth,” and populate the username and password. Then, we create an instance of
										the Web Service as the variable “ws.” We attach the header using the Service’s
										public property AuthHeaderValue, then make the call, and return the results to
										the Label.
									</P>
									<P>If we browse to the Client file through the browser, we will see the results.</P>
									<P>So what we have accomplished, is to create is a Web Service that requires a
										username for authentication. This happens in the real world with web services,
										some require a key string, like the Google API, and others require that you
										have an account.
									</P>
									<P>Thanks for reading and to see the complete code, you can download the source project at http://www.harrisonlogic.com/HL.
									</P>
									<P>Enjoy!</P>
									<P>&nbsp;</P>
								</td>
							</tr>
						</table>

