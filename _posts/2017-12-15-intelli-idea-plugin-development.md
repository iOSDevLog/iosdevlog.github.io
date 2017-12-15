---
layout: post
title: "IntelliJ IDEA Plugin Development"
author: iosdevlog
date: 2017-12-15 17:26:31 +0800
description: ""
category: plugin
tags: [software]
---

<http://www.jetbrains.org/intellij/sdk/docs/basics.html>

Download *IntelliJ IDEA Community* <https://www.jetbrains.com/idea/>


下载 *IntelliJ IDEA Community* <https://www.jetbrains.com/idea/>

## Create Plugin Project

![1](/assets/software/idea/1.png)

![2](/assets/software/idea/2.png)

![3](/assets/software/idea/3.png)

![4](/assets/software/idea/4.png)

* On the New Action page that opens, fill in the following fields, and then click OK:
* Action ID: Enter the unique ID of the action. Recommended format: *PluginName.ID*
* Class Name: Enter the name of the action class to be created.
* Name: Enter the name of the menu item or tooltip for toolbar button associated with action.
* Description: Optionally, enter the action description. The IDEA status bar indicates this description when focusing the action.
* In the Add to Group area, under Groups, Actions and Anchor, specify the action group to which to add a newly created action, and the position of the newly created action relative to other existing actions.
* In the Keyboard Shortcuts area, optionally, specify the first and second keystrokes of the action.

## resources/META-INF/plugin.xml

```
...

<actions>
    <!-- Add your actions here -->
    <action id="IDEAPlugin.IDEAAction" class="IDEAActionClass" text="IDEA_Plugin_Item" 
            description="The Action Implemented By IDEAActionClass class.">
        <add-to-group group-id="MainMenu" anchor="after" relative-to-action="WindowMenu"/>
        <keyboard-shortcut keymap="$default" first-keystroke="ctrl meta alt M"/>
    </action>
</actions>

...
```

Change to 修改为

```
<idea-plugin>
    <id>com.iosdevlog.ideaplugin</id>
    <name>IDEA Plugin Hello World</name>
    <version>1.0</version>
    <vendor email="iosdevlog@iosdevlog.com" url="http://www.iosdevlog.com">iOSDevLog</vendor>

    <description><![CDATA[
      IDEA Plugin Beginner.<br>
      <em>Let's begin to develop a Plugin.</em>
    ]]></description>

    <change-notes><![CDATA[
      First Init.<br>
      <em>also see: http://iosdevlog.com</em>
    ]]>
    </change-notes>

    <!-- please see http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/build_number_ranges.html for description -->
    <idea-version since-build="173.0"/>

    <!-- please see http://www.jetbrains.org/intellij/sdk/docs/basics/getting_started/plugin_compatibility.html
         on how to target different products -->
    <!-- uncomment to enable plugin in all products
    <depends>com.intellij.modules.lang</depends>
    -->

    <extensions defaultExtensionNs="com.intellij">
        <!-- Add your extensions here -->
    </extensions>

    <actions>
        <!-- Add your actions here -->
        <group id="IDEAPlugin.HelloWorld" text="Hello _World Menu"
               description="HelloWorld menu">
            <action id="IDEAPlugin.IDEAAction" class="IDEAActionClass" text="IDEA_Plugin_Item"
                    description="The Action Implemented By IDEAActionClass class.">
                <add-to-group group-id="MainMenu" anchor="after" relative-to-action="WindowMenu"/>
                <keyboard-shortcut keymap="$default" first-keystroke="ctrl meta alt M"/>
            </action>
        </group>
    </actions>

</idea-plugin>
```

## src/IDEAActionClass

```
import com.intellij.openapi.actionSystem.AnAction;
import com.intellij.openapi.actionSystem.AnActionEvent;

public class IDEAActionClass extends AnAction {

    @Override
        public void actionPerformed(AnActionEvent e) {
            // TODO: insert action logic here
        }
}
```

Change to 修改为

```
import com.intellij.openapi.actionSystem.AnAction;
import com.intellij.openapi.actionSystem.AnActionEvent;
import com.intellij.openapi.actionSystem.PlatformDataKeys;
import com.intellij.openapi.project.Project;
import com.intellij.openapi.ui.Messages;

public class IDEAActionClass extends AnAction {
    public IDEAActionClass() {
        super("Hello _World");
    }

    @Override
    public void actionPerformed(AnActionEvent e) {
        Project project = e.getData(PlatformDataKeys.PROJECT);
        String txt= Messages.showInputDialog(project, "What is your name?", "Input Your Name", Messages.getQuestionIcon());
        Messages.showMessageDialog(project, "Hello, " + txt + "!\n I am glad to see you.", "Information", Messages.getInformationIcon());
    }
}
```

## Edit Configurations

![5](/assets/software/idea/5.png)

## Run & Test

![6](/assets/software/idea/6.png)

![7](/assets/software/idea/7.png)

![8](/assets/software/idea/8.png)

![9](/assets/software/idea/9.png)

*Ctrl + Meta + Alt + M* also Worlks!!!
