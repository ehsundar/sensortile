<html>

<head>

</head>

<h1>sensortile</h1>

<p>
sensortile is a multi-sensor device, giving us many physical data such as acceleration, temprature, etc. you can find out more about sensortile on <a href="http://www.st.com/en/evaluation-tools/steval-stlkt01v1.html">official website</a>. there is also an example application (android/ios) for recieving and monitoring sensortile data. the android SDK and example app source code are available on github: <a href="https://github.com/STMicroelectronics-CentralLabs/STBlueMS_Android">here!</a>
</p>
<p>
while trying to use sensortile to catch some physical data (on a platform that's not android) i found out there is no datasheet or GATT table for using sensortile BLE; however there is some standard rules. i will attach a sample python script (uses bluepy) so you will see the raw data.
</p>
here is the GATT table:
<p>
<h3><b>characteristics:</b></h3>
</p>

<div style="overflow-x:scroll;">
<table>
  <tr>
    <th>Feature</th>
    <th>Handle</th> 
    <th>UUID</th>
  </tr>
  <tr>
    <td>ProximityGesture::Motion Intensity</td>
    <td>0x000d</td> 
    <td>00140000-0001-11e1-ac36-0002a5d5c51b</td>
  </tr>
  <tr>
    <td>FeatureAcceleration::FeatureGyroscope::FeatureMagnetometer</td>
    <td>0x0010</td> 
    <td>00e00000-0001-11e1-ac36-0002a5d5c51b</td>
  </tr>
  <tr>
    <td>FeatureAccelerationEvent</td>
    <td>0x0013</td> 
    <td>00000400-0001-11e1-ac36-0002a5d5c51b</td>
  </tr>
  <tr>
    <td>FeatureMicLevel</td>
    <td>0x0016</td> 
    <td>04000000-0001-11e1-ac36-0002a5d5c51b</td>
  </tr>
  <tr>
    <td>FeatureMemsSensorFusionCompact</td>
    <td>0x0019</td> 
    <td>00000100-0001-11e1-ac36-0002a5d5c51b</td>
  </tr>
  <tr>
    <td>FeatureCompass</td>
    <td>0x001c</td> 
    <td>00000040-0001-11e1-ac36-0002a5d5c51b</td>
  </tr>
  <tr>
    <td>FeatureActivity</td>
    <td>0x001f</td> 
    <td>00000010-0001-11e1-ac36-0002a5d5c51b</td>
  </tr>
  <tr>
    <td>FeatureCarryPosition</td>
    <td>0x0022</td> 
    <td>00000008-0001-11e1-ac36-0002a5d5c51b</td>
  </tr>
   <tr>
    <td>FeatureMemsGesture</td>
    <td>0x0025</td> 
    <td>00000002-0001-11e1-ac36-0002a5d5c51b</td>
  </tr>
   <tr>
    <td>FeatureAudioADPCM</td>
    <td>0x0028</td> 
    <td>08000000-0001-11e1-ac36-0002a5d5c51b</td>
  </tr>
   <tr>
    <td>FeatureAudioADPCMSync</td>
    <td>0x002b</td> 
    <td>40000000-0001-11e1-ac36-0002a5d5c51b</td>
  </tr>
</table>
</div>

each characteristic contains 3 description. you must write value 0x0100 to first description of each char (this is a standard method). then sensortile will send you data continuously as notification. you could enable multiple notifications at the same time. notifications will arrive as characteristic value. so you can distinguish notifications.

<ul>
<li>
<b>note</b>
</li>
for two first chars, data has following patern:
2byte<time stamp> 2byte<acc data x> 2byte<acc data y> ... 2byte<mag data pitch>
there is some more info about data structure and uuid prefixes at <a href="https://github.com/STMicroelectronics-CentralLabs/BlueSTSDK_Android/tree/b490d4d382e6f8dddbf9ce6e02414a53d576cd58">SDK git page</a>

<li>
<b>note</b>
</li>
each char data has a time stamp at two first bytes, except stream data (audio)
</ul>
<li>
<b>note</b>
</li>
audio formatting is ADPCM. you cannot find any library to dicode this format to PCM(WAV) (except a corrupt python lib!). so you have two ways: write decode yourself! or use sox library. 
<br>
<p>
<h4>
//end
</h4>
</p>
</html>
