com.erigir.mslinks
=======

## Important note

This is a pure repackaging of the com.erigir.mslinks library forked from https://github.com/BlackOverlord666/com.erigir.mslinks.  All I have
done in this fork is move the code into a Java-standard compliant namespace, and add the wrappings necessary to get 
it into Maven.  Maven packaging looks like (check maven central for newest version number):

```

<dependency>
    <groupId>com.erigir</groupId>
    <artifactId>com.erigir.mslinks</artifactId>
    <version>0.0.2</version>
</dependency>

```

Everything below here is the original documentation.


### Summary
Library for parsing and creating Windows shortcut files (.lnk)

### Description
Partial implementation of [Shell Link (.LNK) Binary File Format](http://msdn.microsoft.com/en-us/library/dd871305.aspx)

This library allows you create new and read and modify existing .lnk files. It can edit most properties of the link such as working directory, tooltip text, icon, command line arguments, hotkeys, create links to LAN shared files and directories but following features are not implemented:

* extra data blocks: Darwin, IconEnvironment, KnownFolder, PropertyStore, Shim, SpecialFolder
* some options in LinkTargetIDList because this section is not documented
* environment variables: you can use it in target path but it resolves while creating link and not stored in the lnk file (the reason is previous item)

### Examples
Easiest way to create link with default parameters: `ShellLink.createLink("targetfile", "linkfile.lnk")`

Following example demonstrates creating link for .bat file and setting working directory, icon and setting up font parameters for console
```
package com.erigir.mslinks;

import java.com.erigir.mslinks.io.IOException;

public class Main {
	public static void main(String[] args) throws IOException {
		ShellLink sl = ShellLink.createLink("pause.bat")
			.setWorkingDir("..")
			.setIconLocation("%SystemRoot%\\system32\\SHELL32.dll");
		sl.getHeader().setIconIndex(128);
		sl.getConsoleData()
			.setFont(com.erigir.mslinks.extra.ConsoleData.Font.Consolas)
			.setFontSize(24)
			.setTextColor(5);
				
		sl.saveTo("testlink.lnk");
		System.out.println(sl.getWorkingDir());
		System.out.println(sl.resolveTarget());
	}
}

```

Final example creates cyclic link that blocks explorer on Windows 7 while trying to get into the containing directory
```
package com.erigir.mslinks;

import java.com.erigir.mslinks.io.IOException;

public class Main {
	public static void main(String[] args) throws IOException {
		ShellLink sl = ShellLink.createLink("test.lnk");
		sl.getHeader().getLinkFlags().setAllowLinkToLink();
		sl.saveTo("test.lnk");
	}
}
```

### Download
* [releases page](https://github.com/BlackOverlord666/mslinks/releases)
* [Maven Central Repository](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.github.vatbub%22%20AND%20a%3A%22mslinks%22)
