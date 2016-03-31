---
Learn how to extend Cortana with more flexible and natural voice commands, so a user can say your app's name anywhere in the command.
Support natural language voice commands in Cortana
ms.assetid: 281E068A-336A-4A8D-879A-D8715C817911
Support natural language voice commands
template: detail.hbs
---

# Support natural language voice commands in Cortana


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Learn how to extend **Cortana** with more flexible and natural voice commands, so a user can say your app's name anywhere in the command.

**Important APIs**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Voice Command Definition (VCD) elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


Extending **Cortana** through voice commands requires the user to indicate both the app and the specific command or function to execute. This is typically accomplished by announcing the app name at the beginning of the voice command. For example, "Adventure Works, add a new trip to Las Vegas."

However, specifying the application name before the command might sound awkward, stilted, or not even make sense. In many cases, being able to say the app name elsewhere in the command is more comfortable and natural, and helps make the interaction much more intuitive and engaging for the user. Our previous example, "Adventure Works, add a new trip to Las Vegas." could be rephrased as "Add a new Adventure Works trip to Las Vegas." or "Add a new trip to Las Vegas using Adventure Works."

You can set up your voice commands to support the app name as a:

-   Prefix - before the command phrase
-   Infix - within the command phrase
-   Suffix - after the command phrase

**Prerequisites:  **

This topic builds on [Launch a background app with voice commands in Cortana](launch-a-background-app-with-voice-commands-in-cortana.md). We continue here to demonstrate features with a trip planning and management app named **Adventure Works**.

If you're new to developing Universal Windows Platform (UWP) apps, have a look through these topics to get familiar with the technologies discussed here.

-   [Create your first app](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Learn about events with [Events and routed events overview](https://msdn.microsoft.com/library/windows/apps/mt185584)

**User experience guidelines:  **

See [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233) for info about how to integrate your app with **Cortana** and [Speech design guidelines](https://msdn.microsoft.com/library/windows/apps/dn596121) for helpful tips on designing a useful and engaging speech-enabled app.

## <span id="Specify_an_AppName_element_in_the_VCD"></span><span id="specify_an_appname_element_in_the_vcd"></span><span id="SPECIFY_AN_APPNAME_ELEMENT_IN_THE_VCD"></span>Specify an **AppName** element in the VCD


The **AppName** element is used to specify a user-friendly name for an app in a voice command.

```XML
<AppName>Adventure Works</AppName></code></pre></td>
</tr>
</tbody>
</table>
```

## <span id="Specify_where_the_app_name_can_be_spoken_in_the_voice_command"></span><span id="specify_where_the_app_name_can_be_spoken_in_the_voice_command"></span><span id="SPECIFY_WHERE_THE_APP_NAME_CAN_BE_SPOKEN_IN_THE_VOICE_COMMAND"></span>Specify where the app name can be spoken in the voice command


The **ListenFor** element has a **RequireAppName** attribute that specifies where the app name can appear in the voice command. This attribute supports four values.

1.  **BeforePhrase**

    Default.

    Indicates that users must say your app name before the command phrase.

    Here, Cortana listens for "Adventure Works when is my trip to Las Vegas".

    <span codelanguage="XML"></span>
```    XML
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">XML</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
<ListenFor RequireAppName="BeforePhrase"> when is [my] trip to {destination} </code></pre></td>
    </tr>
    </tbody>
    </table>
```

2.  **AfterPhrase**

    Indicates that users must say your app name after the command phrase.

    A localized phrase list of prepositional conjunctions is provided by the system. This includes phrases such as, "using", "with "and "on".

    Here, Cortana listens for commands like "Show my next trip to Las Vegas on Adventure Works" and "Show my next trip to Las Vegas using Adventure Works".

    <span codelanguage="XML"></span>
```    XML
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">XML</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
<ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor></code></pre></td>
    </tr>
    </tbody>
    </table>
```

3.  **BeforeOrAfterPhrase**

    Indicates that users must say your app name either before or after the command phrase.

    For the suffix version, a localized phrase list of prepositional conjunctions is provided by the system. This includes phrases such as, "using", "with "and "on".

    Here, Cortana listens for commands like "Adventure Works, show my next trip to Las Vegas" or "Show my next trip to Last Vegas on Adventure works".

    <span codelanguage="XML"></span>
```    XML
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">XML</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
<ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination} </ListenFor></code></pre></td>
    </tr>
    </tbody>
    </table>
```

4.  **ExplicitlySpecified**

    Indicates that users must say your app name exactly where you specify in the command phrase. The user is not required to say the app name either before or after the phrase.

    You must explicitly reference your app name using the **{builtin:AppName}** tag.

    Here, Cortana listens for commands like "Adventure Works, show my next trip to Las Vegas" or "Show my next Adventure Works trip to Las Vegas".

    <span codelanguage="XML"></span>
```    XML
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">XML</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
<ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor></code></pre></td>
    </tr>
    </tbody>
    </table>
```

## <span id="Special_cases"></span><span id="special_cases"></span><span id="SPECIAL_CASES"></span>Special cases


When you declare a **ListenFor** element where **RequireAppName** is either "AfterPhrase" or "ExplicitlySpecified", you must ensure certain requirements are met:

1.  **{builtin:AppName}** must appear once and only once when **RequireAppName** is "ExplicitlySpecified".

    With this value, the system cannot infer where the app name can appear in the voice command. You must explicitly specify the location.

2.  You cannot have a voice command begin with a **PhraseTopic** element, which is typically used for large vocabulary speech recognition. At least one word must precede it.

    This helps to minimize the chance that **Cortana** launches your app if a command contains your app name, or part of it, anywhere in the utterance.

    Here is an invalid declaration that could lead to **Cortana** launching the **Adventure Works** app if the user says something like "Show me reviews for Kinect adventure works".

    <span codelanguage="XML"></span>
```    XML
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">XML</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
<ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor></code></pre></td>
    </tr>
    </tbody>
    </table>
```

3.  There must be at least two words in the **ListenFor** string besides your app name and references to **PhraseTopic** elements.

    Similar to case 2, you need to ensure your commands contain sufficient phonetic content to minimize the chances your app is launched unintentionally.

    This helps you set up your application for best possible success so your application does not get incorrectly launched when user says for example “Find Kinect Adventure works”.

    Here are invalid declarations that could lead to **Cortana** launching the **Adventure Works** app if the user says something like "Hey adventure works" or "Find Kinect adventure works".

    <span codelanguage="XML"></span>
```    XML
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">XML</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
<ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
    <ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Remarks


Supporting more variation in how a voice command can be uttered by a user in **Cortana** also increases the general usability of your app.

Avoid having "Hey \[app name\]" as your **AppName** or **CommandPrefix**. Users are much more likely to say "Hey Cortana" to invoke Cortana through voice activation, and having "Hey \[app name\]" in the utterance does not sound natural. For example, "Hey Cortana, show my next trip to Las Vegas on Hey Adventure Works".

Consider adding infix/suffix variations to your existing voice commands. As we've shown here, it doesn't require a lot of effort to add an additional attribute to your existing **ListenFor** elements and support suffix variants. It feels a lot more natural to say "Hey Cortana, show my next trip to Las Vegas on Adventure works" than "Hey Cortana, Adventure Works, show my next trip to Las Vegas".

Consider using your app name as a prefix in cases where the voice command conflicts with existing **Cortana** functionality (calling, messaging, and so on). For example, "Adventure Works, message \[travel agent\] about Las Vegas trip".

## <span id="Complete_example"></span><span id="complete_example"></span><span id="COMPLETE_EXAMPLE"></span>Complete example


Here is a VCD file that demonstrates various ways to provide more natural language voice commands.

**Note**  It is valid to have multiple **ListenFor** elements, each with a different **RequireAppName** attribute value.

 

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="commandSet_en-us">
    <AppName>Adventure Works</AppName>
    <Example> When is my trip to Las Vegas? </Example>
    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforePhrase">
        when is my] trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands like 
           “Show my next trip to Las Vegas on Adventure Works”; “Show my next 
           trip to Las Vegas using Adventure Works” -->
      <ListenFor RequireAppName="AfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands when 
           the user specifies your app name either before or after the command. 
           “Adventure Works, show my next trip to Las Vegas”; 
           “Show my next trip to Last Vegas on Adventure works” -->
      <ListenFor RequireAppName="BeforeOrAfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands 
           when the user specifies your app name inline. 
           “Show my next Adventure Works trip to Las Vegas” -->
      <ListenFor RequireAppName="ExplicitlySpecified">
        show [my] next {builtin:AppName} trip to {destination} </ListenFor>

      <Feedback> Looking for trip to {destination} </Feedback>
      <Navigate />
    </Command>
    <PhraseList Label="destination">
      <Item> Las Vegas </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>
  </CommandSet>
  <!-- Other CommandSets for other languages -->
</VoiceCommands>
```

## <span id="related_topics"></span>Related articles


**Developers**
* [Cortana interactions](cortana-interactions.md)
* [Define custom recognition constraints](define-custom-recognition-constraints.md)
* [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designers**
* [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Speech design guidelines](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Samples**
* [Cortana voice command sample](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 




<!--HONumber=Mar16_HO1-->
