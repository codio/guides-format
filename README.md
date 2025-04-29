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
	// boolean, allow you to hide "Mark as Complete" option for student, seesubmit options - https://docs.codio.com/instructors/authoring/assessments/student-submission.html#student-submission
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
	// array, pages/chapters/sections order on current level, should be omited for pages 
	"order": [
		"New-Page-05b2"
	],
	// array, see files section
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
	"learningObjectives": "some text"
}
```
