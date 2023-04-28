---
title: "MSci project"
toc: true
toc_sticky: true
header:
    overlay_image: /images/msci.png
    overlay_filter: 0.2
    actions:
    - label: "Report"
      url: "/files/Ronan_Laker_Msci_Report.pdf"
excerpt: "Project aimed to predict the solar wind conditions that would be experienced by Parker Solar Probe on its first encounter with the Sun"
---



This ended up being part of the first results paper published in nature. This work resulted in a co-authorship on the [FIELDS instrument's first result paper](https://www.nature.com/articles/s41586-019-1818-7).

<style> 
.custombutton,
.custombutton:visited{
    border-radius: 8px;
    font-size: 20px;
    margin-bottom:5px;
    padding-top:0px;
    padding-bottom:0px;
    padding-left:15px;
    padding-right:15px;
    height: 4em;
    border: 2px solid black;
    color: black;
    text-decoration: none;
    text-align: center;  
}

.custombutton:hover {
    background-color: black;
    color: white;
    text-decoration: none;
}

.buttonpdf,
.buttonpdf:visited {
    background-color: white; 
    color: #c90241; 
    border: 2px solid #c90241;
}

.buttonpdf:hover {
    background-color: #c90241;
    color: white;
}

.buttonposter,
.buttonposter:visited {
    background-color: white; 
    color: blue;
    border: 2px solid  blue;
}

.buttonposter:hover {
    background-color: blue;
    color: white;
}
</style>


<a class="custombutton buttonpdf" href="/files/Ronan_Laker_Msci_Report.pdf" target="_blank" rel="noopener noreferrer" >Report</a> <a class="custombutton buttonposter" href="/files/Ronan_Laker_Tom_Woolley_Poster.pdf" target="_blank" rel="noopener noreferrer" >Poster</a>


## Videos

<iframe width="560" height="315" src="https://www.youtube.com/embed/GgJgghScZCc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<span style="font-size:16px;">
    The color gradient shows the speed of the simulated solar wind (using WSA-ENLIL model) with a more yellow colour indicating a faster solar wind. Top left shows the solar wind and Parker Solar Probe orbit in an inertial frame, whereas the top right is the equivalent but in a frame corotating with the Sun's surface. Parker Solar Probe is shown by the white dot, and Earth by the green dot. The graph underneath shows the predicted velocity Parker Solar Probe was predicted to see for its first orbit in November 2018.
</span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/G5ERYXIiIp8" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<span style="font-size:16px">
    Top left image is from the STEREO-A spacecraft, and the top right is from the SDO spacecraft ahead of Earth. The field of view of these images is shown by the shaded regions of the bottom graph, which is a magnetic map of the Sun's entire surface. Parker Solar Probe had its first closest approach in November 2018, and this links features such as coronal holes and active regions to the in situ data measured by Parker Solar Probe. The magnetic field line connecting to Parker Solar Probe is shown on the bottom graph and the relevant image to show the source region of the solar wind.*
</span>

<a name="summary"></a>
## Lay Person Summary

The night sky is filled with a billion-trillion stars, although most can only be seen with powerful telescopes. Given their abundance it is essential that we study them to further our own knowledge of the universe. But stars are far away, and it would take thousands of years to reach them with a spacecraft. Well, lucky for us, there happens to be a star right on our front doorstep: the Sun.

Like all stars, the Sun is a large ball of hot gas, so hot in fact, that the electrons and protons in atoms become separated, creating a fourth state of matter, known as plasma. This plasma cannot always be contained by the Sun’s enormous weight, and streams outwards in all directions. This flow of charged particles, called the solar wind, barrels past the planets at speeds around 500km/s. “But I thought space was empty?” I hear you cry. Well, although there is a flow of charged particles, their density is only around 6 particles per cm3, which is practically nothing compared with 100 per cm3 in the best laboratory vacuum.

The Earth is protected from these charged particles by its magnetic field, which acts as a barrier that the wind flows around. However, sometimes the solar wind conditions mean that particles can make it through this barrier, driving effects such as the Northern and Southern Lights. But it’s not all pretty light displays. The solar wind can occasionally drive space weather at Earth, which has the potential to destroy power grids, GPS and global communications. This has the potential to cause trillions of pounds worth of damage, so it is extremely important that we understand how the solar wind is formed and accelerates towards Earth.

We have learnt a few things about the solar wind since the start of the space age, such as the fact it is mainly made of slow and fast streams that seem to originate from different regions on the Sun. Scientists have had to make do with spacecraft data from the 1970s, that only had a memory of 500kb, as this previously held the record for closest distance to the Sun. Such out-dated technology means that there are some glaring omissions in our knowledge. For example, the Sun’s corona (no, not the Mexican beer, the outer layer of the Sun’s atmosphere) has a temperature of 2 million°C, but the surface of the Sun only has a temperature of around 6000°C. That’s like walking closer to a fire and getting colder, so there must be another source of energy that we have not discovered yet…

Mysteries such as this one provided NASA with the motivation to spend $1.5bn on the Parker Solar Probe (Probe), which launched in August 2018. Probe has already become the fastest man-made object ever created, travelling at a speed of 247,000 km/h, meaning it could travel the length of the UK in just 13 seconds! The aim of this mission is to ‘touch the Sun’, by getting as close as 6.9 million km away from the Sun. While this does not sound all that close, it will mean literally flying through the corona.

You might ask yourself ‘Has someone thought about what solar wind conditions Probe might experience on its first orbit?’. Well you’d think so wouldn’t you? But apparently not `(edit: at the time)`, and that’s where my master’s project comes in. I provided predictions of what solar wind speed, density and temperature Probe would experience on its first orbit, before the science teams had seen the data. I achieved this by running a large simulation of the solar wind on a supercomputer, which I then ‘flew’ Probe through in order to get a time series of the conditions it would experience. This was important as Probe was too far away from Earth to download all the data in December (the rest will be retrieved in April), so the science team had to select the parts they wanted, and my predictions helped give some idea of what they were downloading. My work is also important when testing theories on how the solar wind is accelerated, as it allows for a connection to be made between the plasma measured by the spacecraft and features seen on the Sun. My project partner and I are privileged enough to have access to the magnetic field data from Probe, something that very few people in the world can say!

Probe represents a new era for space physics and will make revolutionary discoveries during its 7-year lifetime. So, who knows maybe you too will work on this ground-breaking mission.
