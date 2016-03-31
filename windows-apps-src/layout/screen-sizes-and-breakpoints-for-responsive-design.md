---
'Screen sizes and break points for responsive design'
.
ms.assetid: BF42E810-CDC8-47D2-9C30-BAA19DCBE2DA
Screen sizes and break points
template: detail.hbs
---

#  Screen sizes and break points for responsive design


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]



The number of device targets and screen sizes across the Windows 10 ecosystem is too great to worry about optimizing your UI for each one. Instead, we recommended designing for a few key widths (also called "breakpoints"): 320, 720, and 1024 epx.

**Tip**  When designing for specific breakpoints, design for the amount of screen space available to your app (the app's window). When the app is running full-screen, the app window is the same size of the screen, but in other cases, it's smaller.

 

This table describes the different size classes and provides general recommendations for tailoring for those size classes.

![responsive design breakpoints](images/rsp-design/rspd-breakpoints.png)

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Size class</th>
<th align="left">small</th>
<th align="left">medium</th>
<th align="left">large</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Width in effective pixels</td>
<td align="left">320</td>
<td align="left">720</td>
<td align="left">1024</td>
</tr>
<tr class="even">
<td align="left">Typical screen size (diagonal)</td>
<td align="left">4' to 6'</td>
<td align="left">6+&quot; to 12&quot;</td>
<td align="left">13' and wider</td>
</tr>
<tr class="odd">
<td align="left">Typical devices</td>
<td align="left">Phones</td>
<td align="left">Tablet, phones with large screen</td>
<td align="left">PCs, laptops, Surface Hubs</td>
</tr>
<tr class="even">
<td align="left">General recommendations</td>
<td align="left"><ul>
<li>You can make navigation and interactions easier on hand-held devices by placing navigation and command elements at the bottom of the screen so that users can easily reach them with their thumbs.</li>
<li>Center tab elements.</li>
<li>Set left and right window margins to 12 px to create a visual separation between the left and right edges of the app window.</li>
<li>Use 1 column/region at a time</li>
<li>Use an icon to represent search (don't show a search box).</li>
<li>Put the navigation pane in overlay mode to conserve screen space.</li>
<li>If you're using the master detail element, use the stacked presentation mode to save screen space.</li>
<li><p>With Continuum for Phones, a new experience for compatible Windows 10 mobile devices, users can connect their phones to a monitor and even use a mouse and keyboard to make their phones work like a laptop. (For more info, see the [Continuum for Phone article](http://go.microsoft.com/fwlink/p/?LinkID=699431).)</p></li>
</ul></td>
<td align="left"><ul>
<li>Make tab elements left-aligned.</li>
<li><p>Set left and right window margins to 24 px.</p>
<p>We recommend creating a visual separation between the left and right edges of the app window.</p></li>
<li>Up to two columns/regions</li>
<li>Show the search box.</li>
<li>Put the navigation pane into docked mode so that it always shows.</li>
</ul></td>
<td align="left"><ul>
<li>Put navigation and command elements at the top of the app window.</li>
<li>Make tab elements left-aligned.</li>
<li><p>Set left and right window margins to 24 px.</p>
<p>We recommend creating a visual separation between the left and right edges of the app window.</p></li>
<li>Up to three columns/regions</li>
<li>Show the search box.</li>
<li>Put the navigation pane into docked mode so that it always shows.</li>
</ul></td>
</tr>
</tbody>
</table>


 




<!--HONumber=Mar16_HO1-->
