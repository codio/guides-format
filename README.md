# guides-format
## Guides format description

`.guides/content` structure
```
- content
    - new-chapter-1111
      - index.json
      - index.md
      - page-2222.json
      - page-2222.md
      - new-section
        - index.json
    - index.json
    - new-page-3333.json
    - new-page-3333.html
    - footer.html
    - header.html
```

`header.html` and `footer.html` - The header/footer will be placed before/after the HTML page content.

`./guides/content/index.json` - root metadata file that contains guides global settings
```
{
	// string, guides title that appears in TOC
	"title": "guides metadata",
	// string, guides theme, only 'light' for now
	"theme": "light",
	// array, allows you to point to one or more .js files in your project (usually located within the .guides folder) that is run when the page is shown. This is especially useful when interacting with a button in a page of content.
	"scripts": ["some-script.js"],
	// string, if you use this option, an icon will appear in the toolbar that will load the Lexikon window with the selected topic automatically selected. Students can still access the Lexicon from the **Tools>Lexicon** menu (unless of course you are restricting the top menu available to them)
	"lexikonTopic": "topic",
	// boolean, allows you to suppress the section page numbers when in Play Mode.
	"suppressPageNumbering": true,
	// boolean, allow you to hide submit buttons for assessments, see submit option - https://docs.codio.com/instructors/authoring/assessments/student-submission.html#student-submission
	"useSubmitButtons": true,
	// boolean, allow you to hide "Mark as Complete" option for student, see submit options - https://docs.codio.com/instructors/authoring/assessments/student-submission.html#student-submission
	"useMarkAsComplete": true,
	// boolean, allows you to hide the main Codio menu items in the IDE (Codio/Project/File/Edit etc) when the assignment is run in a course.
	"hideMenu": true,
	// boolean, allows students to be able to close the content. It can be restarted by selecting the Start icon in the file tree
	"allowGuideClose": true,
	// boolean, starts the assignment with the guides pane collapsed. Students can show the content by clicking on the index icon on the right
	"collapsedOnStart": true,
	// boolean, hides the sections list in your content for the students.
	"hideSectionsToggle": true,
	// boolean, hides this button that would otherwise show on the last page of the guides.
	"hideBackToDashboard": true,
	// boolean, prevents students from closing files in tabs.
	"protectLayout": true,
	// array, pages/chapters/sections order on current level
	"order": [
		"Page-1-52c5",
		"New-Page-c6f8",
		"New-Chapter-1c63"
	]
}
```

Each `page/section/chapter` metadata `.json` file contains `id` and `title`. node file name should be 
```
const escape = (name || '').replace(/[^a-z0-9]/gi, '-')
const fileName = `${escape(node.title)}-${node.id.substring(0, 4)}`
```

file structure for page
```
  page-title-1234.json
  page-title-1234.md
```
file structure for `section/chapter`
```
  - section-title-2233
    - index.json
    - index.md
```
content can be `.md` or `.html`. Content type should be defined in the `.json` file

`section/chapter` without content `json` file format
```
{
	// string, uniq id
	"id": "1c639496-6450-3734-7b8d-34dddfde7efe",
	// string, node type, can be 'section' or 'chapter'
	"type": "chapter",
	// string, node title
	"title": "New Chapter",
	// array, pages/chapters/sections order on current level
	"order": [
		"New-Section-80ee"
	]
}
```
`page/section/chapter` with content `.json` file format
```
{
	// string, uniq id
	"id": "80eee47c-59d2-d045-5330-e524d940d7bc",
	// string, node title
	"title": "New Section",
	// string, node type, can be 'page', 'section' or 'chapter'
	"type": "section",
	// array, pages/chapters/sections order on current level, should be omitted for pages 
	"order": [
		"New-Page-05b2"
	],
	// array, see "Actions" section
	"files": [
		{
			"path": "#tabs",
			"action": "close"
		},
		{
			"path": "index.html",
			"panel": 0,
			"action": "open"
		},
		{
			"path": "#terminal: ls",
			"panel": 1,
			"action": "open"
		}
	],
	// string, allows you to specify the number of panels/columns you want to choose for this section, or set to **Previous** to inherit the layout settings from the previous page, see layout section
	"layout": "2-panels-tree-guides-left",
	// array, allows you to define specific folders in your project that you wish to be visible when the current section is displayed
	"path": [
		"folderToShow"
	],
	// string, content type, can be 'md' or 'html'
	"contentType": "html",
	// boolean, allows you to show content that only teachers are able to see.
	"teacherOnly": true,
	// boolean, when Close tabs enabled, allows you the option to retain terminal session from previous section. By default, terminal session will close
	"closeTerminalSession": true,
	// string, allows you to define learning objectives for this section
	"learningObjectives": "some text",
	// media element, allows you to define audio element and audio action for the page
	"media": {
		// media element type. can be "audio"
		"type": "audio",
		// string, media element name
		"name": "test audio",
		// string, path to audio file
		"source": "file_example_MP3_1MG.mp3",
		// boolean, you to enable/disable the page audio element 
		"disabled": true,
		// array, see "Media Actions" section
		"actions": [
			{
				"action": "open",
				"tabType": "file",
				"time": 8,
				"type": "file_open",
				"content": "test.js",
				"panel": 0,
				"lines": null
			},
		]
	}

}
```

## Actions
https://docs.codio.com/instructors/authoring/guides/settings/opentabs.html

### Close all tabs
```
{
	"path": "#tabs",
	"action": "close"
}
```

### Open file
```
{
	"path": "test.js", // string, file path to open
	"panel": 0, // number, panel index to open
	"action": "open"
},
```

### Open preview
```
{
	"path": "#preview: https://google.com",
	"panel": 0, // number, panel index to open
	"action": "open"
},
```

### Open terminal window and run command(optionally)
```
{
	"path": "#terminal: ls", // string, "#terminal: {command_to_run}"
	"panel": 0, // number, panel index to open
	"action": "open"
},
```

### Open file and highlight text
```
{
	"path": "test.js", // string, file path to open
	"panel": 0, // number, panel index to open
	"ref": "some marker text", // string, first line to highlight reference
	"lineCount": 3, // number, lines count to highlight
	"action": "open"
},
```

### Open visualizer (PythonTutor)
```
{
	"path": "#tutor: test.js", // string, file path to open, "#tutor: {path}"
	"panel": 0, // number, panel index to open
	"action": "open"
},
```

### Open VM (VM or VM SSH)
vm: 
```
{
	"path": "#vm: ",
	"panel": 0, // number, panel index to open
	"action": "open"
},
```
vm ssh:
```
{
	"path": "#vmssh: ",
	"panel": 0, // number, panel index to open
	"action": "open"
},
```

### Cyber Range
```
{
	"path": "#vmnested: KaliLinux", // string, "#vmnested: {target_machine}"
	"panel": 0, // number, panel index to open
	"action": "open"
},
```
### Open earsketch
```
{
	"path": "#earsketch: sample.py", // string, "#earsketch: {path}"
	"panel": 0, // number, panel index to open
	"action": "open"
},
```
### Open jupyter
```
{
	"path": "#jupyter-lab: test.ipynb", // string, "#jupyter-lab: {path}"
	"panel": 0, // number, panel index to open
	"action": "open"
}
```

## Media Actions

### Open/Close file

open:
```
{
	"action": "open",
	"tabType": "file",
	"time": 8, // number, start action time in seconds
	"type": "file_open",
	"content": "test.js", // string, file path to open
	"panel": 0, // number, panel index to open
},
```

close:
```
{
	"action": "close",
	"tabType": "file",
	"time": 12, // number, start action time in seconds
	"type": "file_close",
	"content": "test.js", // string, file path to close
	"panel": 0 // number, panel index to close
},
```

### Open/Close terminal and run command

open: 
```
{
	"action": "open",
	"tabType": "terminal",
	"time": 13, // number, start action time in seconds
	"type": "terminal_open",
	"panel": 0 // number, panel index to open
},
```
close:
```
{
	"action": "close",
	"tabType": "terminal",
	"time": 13, // number, start action time in seconds
	"type": "terminal_close"
},
```
run command:
```
{
	"action": "open",
	"tabType": "terminal",
	"time": 14, // number, start action time in seconds
	"type": "run_command",
	"content": "ls", // string, command to run
	"panel": 1 // number, panel index to open
},
```

### Highlight code
```
{
	"action": "open",
	"tabType": "file",
	"time": 46, // number, start action time in seconds
	"type": "highlight",
	"content": "test.js", // string, file path to open
	"panel": 0, // number, panel index to open
	"reference": "line 1", // string, first line to highlight reference
	"lines": 3 // number, lines count to highlight
},
```

### Close all tabs
```
{
	"time": 47, // number, start action time in seconds
	"type": "close_all"
},
```

### Pause
```
			{
	"time": 58, // number, start action time in seconds
	"type": "pause"
}
```