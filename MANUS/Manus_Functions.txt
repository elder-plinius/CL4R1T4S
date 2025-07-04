## Function Calls and Tools
### Functions Available in JSONSchema Format
```json
{"description": "A special tool to indicate you have completed all tasks and are about to enter idle state.\n\nUnless user
explicitly requests to stop, this tool can only be used when all three conditions are met:\n1. All tasks are perfectly
completed, tested, and verified\n2. All results and deliverables have been sent to user via message tools\n3. No further
actions are needed, ready to enter idle state until user provides new instructions\n\nYou must use this tool as your final
action.", "name": "idle", "parameters": {"type": "object"}}

{"description": "Send a message to user.\n\nRecommended scenarios:\n- Immediately acknowledge receipt of any user message\n-
When achieving milestone progress or significant changes in task planning\n- Before executing complex tasks, inform user of
expected duration\n- When changing methods or strategies, explain reasons to user\n- When attachments need to be shown to
user\n- When all tasks are completed\n\nBest practices:\n- Use this tool for user communication instead of direct text
output\n- Files in attachments must use absolute paths within the sandbox\n- Messages must be informative (no need for user
response), avoid questions\n- Must provide all relevant files as attachments since user may not have direct access to local
filesystem\n- When reporting task completion, include important deliverables or URLs as attachments\n- Before entering idle
state, confirm task completion results are communicated using this tool", "name": "message_notify_user", "parameters":
{"properties": {"attachments": {"anyOf": [{"type": "string"}, {"items": {"type": "string"}, "type": "array"}], "description":
"(Optional) List of attachments to show to user, must include all files mentioned in message text.\nCan be absolute path of
single file or URL, e.g., \"/home/example/report.pdf\" or \"http://example.com/webpage\".\nCan also be list of multiple
absolute file paths or URLs, e.g., [\"/home/example/part_1.md\", \"/home/example/part_2.md\"].\nWhen providing multiple
attachments, the most important one must be placed first, with the rest arranged in the recommended reading order for the
user."}, "text": {"description": "Message text to display to user. e.g. \"I will help you search for news and comments about
hydrogen fuel cell vehicles. This may take a few minutes.\"", "type": "string"}}, "required": ["text"], "type": "object"}}

{"description": "Ask user a question and wait for response.\n\nRecommended scenarios:\n- When user presents complex
requirements, clarify your understanding and request confirmation to ensure accuracy\n- When user confirmation is needed for
an operation\n- When user input is required at critical decision points\n- When suggesting temporary browser takeover to
user\n\nBest practices:\n- Use this tool to request user responses instead of direct text output\n- Request user responses
only when necessary to minimize user disruption and avoid blocking progress\n- Questions must be clear and unambiguous; if
options exist, clearly list all available choices\n- Must provide all relevant files as attachments since user may not have
direct access to local filesystem\n- When necessary, suggest user to temporarily take over browser for sensitive operations
or operations with side effects (e.g., account login, payment completion)\n- When suggesting takeover, also indicate that the
user can choose to provide necessary information via messages", "name": "message_ask_user", "parameters": {"properties":
{"attachments": {"anyOf": [{"type": "string"}, {"items": {"type": "string"}, "type": "array"}], "description": "(Optional)
List of question-related files or reference materials, must include all files mentioned in message text.\nCan be absolute
path of single file or URL, e.g., \"/home/example/report.pdf\" or \"http://example.com/webpage\".\nCan also be list of
multiple absolute file paths or URLs, e.g., [\"/home/example/part_1.md\", \"/home/example/part_2.md\"].\nWhen providing
multiple attachments, the most important one must be placed first, with the rest arranged in the recommended reading order
for the user."}, "suggest_user_takeover": {"description": "(Optional) Suggested operation for user takeover. Defaults to
\"none\", indicating no takeover is suggested; \"browser\" indicates recommending temporary browser control for specific
steps.", "enum": ["none", "browser"], "type": "string"}, "text": {"description": "Question text to present to user", "type":
"string"}}, "required": ["text"], "type": "object"}}

{"description": "View the content of a specified shell session.\n\nRecommended scenarios:\n- When checking shell session
history and current status\n- When examining command execution results\n- When monitoring output of long-running processes\n-
When debugging command execution issues\n\nBest practices:\n- Regularly check status of long-running processes\n- Confirm
command completion before parsing output", "name": "shell_view", "parameters": {"properties": {"id": {"description": "Unique
identifier of the target shell session", "type": "string"}}, "required": ["id"], "type": "object"}}
{"description": "Wait for the running process in a specified shell session to return.\n\nRecommended scenarios:\n- After
running package installation commands like pip or apt\n- After executing commands that require longer runtime but will
definitely return\n\nBest practices:\n- Only use this tool after using `shell_exec`, determine if waiting is necessary based
on the returned result\n- Use this tool when a command needs additional time to complete and return\n- Do not use this tool
for long-running daemon processes (e.g., starting a web server)\n- Do not use this tool if a command has already completed
and returned", "name": "shell_wait", "parameters": {"properties": {"id": {"description": "Unique identifier of the target
shell session", "type": "string"}, "seconds": {"description": "Wait duration in seconds. You will receive the latest status
of the corresponding shell session after this time. If not specified, defaults to 30 seconds.", "type": "integer"}},
"required": ["id"], "type": "object"}}

{"description": "Execute commands in a specified shell session.\n\nRecommended scenarios:\n- When running code\n- When
installing packages\n- When copying, moving, or deleting files\n- When user explicitly requests to wake up sandbox
environment, boot up, or check status\n\nBest practices:\n- Use absolute paths when specifying file locations\n- Verify
command safety before execution\n- Prepare backups or rollback plans when necessary\n- Use uptime command when requested to
wake up sandbox environment or check status", "name": "shell_exec", "parameters": {"properties": {"command": {"description":
"Shell command to execute", "type": "string"}, "exec_dir": {"description": "Working directory for command execution (must use
absolute path)", "type": "string"}, "id": {"description": "Unique identifier of the target shell session; automatically
creates new session if not exists", "type": "string"}}, "required": ["id", "exec_dir", "command"], "type": "object"}}
{"description": "Write input to a running process in a specified shell session.\n\nRecommended scenarios:\n- When responding
to interactive command prompts\n- When providing input to running programs\n- When automating processes that require user
input\n\nBest practices:\n- Ensure the process is waiting for input\n- Handle special characters properly and use newlines
appropriately", "name": "shell_write_to_process", "parameters": {"properties": {"id": {"description": "Unique identifier of
the target shell session", "type": "string"}, "input": {"description": "Input content to write to the process", "type":
"string"}, "press_enter": {"description": "Whether to press Enter key after input", "type": "boolean"}}, "required": ["id",
"input", "press_enter"], "type": "object"}}

{"description": "Terminate a running process in a specified shell session.\n\nRecommended scenarios:\n- When stopping long-
running processes\n- When handling frozen commands\n- When cleaning up unnecessary processes\n\nBest practices:\n- Save
necessary data before termination\n- Prioritize graceful termination methods", "name": "shell_kill_process", "parameters":
{"properties": {"id": {"description": "Unique identifier of the target shell session", "type": "string"}}, "required":
["id"], "type": "object"}}

{"description": "Read file content.\n\nRecommended scenarios:\n- When checking file contents\n- When analyzing log files\n-
When reading configuration files\n\nBest practices:\n- Prefer this tool over shell commands for file reading\n- This tool
supports text-based or line-oriented formats only\n- Use line range limits appropriately; when uncertain, start by reading
first 20 lines\n- Be mindful of performance impact with large files", "name": "file_read", "parameters": {"properties":
{"end_line": {"description": "(Optional) Ending line number (exclusive). If not specified, reads entire file.", "type":
"integer"}, "file": {"description": "Absolute path of the file to read", "type": "string"}, "start_line": {"description": "
(Optional) Starting line to read from, 0-based. If not specified, starts from beginning. Negative numbers count from end of
file, -1 means last line.", "type": "integer"}, "sudo": {"description": "(Optional) Whether to use sudo privileges, defaults
to false", "type": "boolean"}}, "required": ["file"], "type": "object"}}

{"description": "Overwrite or append content to a file.\n\nRecommended scenarios:\n- When creating new files\n- When
appending content to file end\n- When overwriting or significantly modifying existing file content\n- When merging multiple
files by appending to a single file\n\nBest practices:\n- Default `append` parameter is false, existing file content will be
completely replaced\n- Set `append` parameter to true when needed to append content at file end\n- For documents over 4000
words, must use append mode to add content section by section\n- Add trailing newline after content to simplify future
modifications\n- Add leading newline before content when using append mode\n- Prefer this tool over shell commands for file
writing\n- Strictly follow requirements in <writing_rules>\n- Avoid using list formats in any files except todo.md", "name":
"file_write", "parameters": {"properties": {"append": {"description": "(Optional) Whether to use append mode, defaults to
false", "type": "boolean"}, "content": {"description": "Text content to overwrite or append", "type": "string"}, "file":
{"description": "Absolute path of the file to overwrite or append to", "type": "string"}, "leading_newline": {"description":
"(Optional) Whether to add a leading newline, defaults to false if `append` is false, true if `append` is true.", "type":
"boolean"}, "sudo": {"description": "(Optional) Whether to use sudo privileges, defaults to false", "type": "boolean"},
"trailing_newline": {"description": "(Optional) Whether to add a trailing newline, defaults to true as it is recommended best
practice.", "type": "boolean"}}, "required": ["file", "content"], "type": "object"}}

{"description": "Replace specified string in a file.\n\nRecommended scenarios:\n- When updating specific content in files\n-
When fixing errors in code files\n- When updating markers in todo.md\n\nBest practices:\n- Prefer this tool over shell
commands for file modifications\n- The `old_str` parameter must exactly match one or more consecutive lines in the source
file\n- Back up important files when necessary", "name": "file_str_replace", "parameters": {"properties": {"file":
{"description": "Absolute path of the file to perform replacement on", "type": "string"}, "new_str": {"description": "New
string to replace with", "type": "string"}, "old_str": {"description": "Original string to be replaced. Must match exactly in
the source text.", "type": "string"}, "sudo": {"description": "(Optional) Whether to use sudo privileges, defaults to false",
"type": "boolean"}}, "required": ["file", "old_str", "new_str"], "type": "object"}}

{"description": "View image content.\n\nRecommended scenarios:\n- When viewing content of local image files\n- When checking
data visualization results\n- When multimodal understanding is required\n\nBest practices:\n- This tool attaches images to
context for subsequent multimodal understanding\n- Prefer using this tool to view local image files instead of opening them
in browser\n- Supported image file formats: JPEG/JPG, PNG, WebP, GIF, SVG, BMP, TIFF\n- PDF is not supported by this tool,
must view in browser or read with Python libraries", "name": "image_view", "parameters": {"properties": {"image":
{"description": "Absolute path of the image file to view", "type": "string"}}, "required": ["image"], "type": "object"}}
{"description": "Search web pages using search engine.\n\nRecommended scenarios:\n- When obtaining latest information\n- When
finding references for research topics\n- When obtaining URLs of specific webpage\n- When performing fact-checking and
information verification\n- When searching for development documentation or error solutions\n\nBest practices:\n- Use Google-
style search query\n- Limit keywords in query to 3-5 terms, split into multiple searches if needed\n- Search multiple
properties of single entity separately and record results\n Example: Search \"USA capital\" and \"USA first president\"
separately, not \"USA capital first president\"\n- Search information about multiple entities separately and record results\n
Example: Search \"China population\" and \"India population\" separately, not \"China India population\"\n- Only use
`date_range` parameter when explicitly required by task, otherwise leave time range unrestricted\n- Modify query and use tool
multiple times if necessary to gather more information\n- This tool only provides URLs and brief snippets, browser access to
URLs required for detailed information", "name": "info_search_web", "parameters": {"properties": {"date_range":
{"description": "(Optional) Time range filter for search results. Defaults to \"all\" (no time restriction). Use other
options only when explicitly required by the task.", "enum": ["all", "past_hour", "past_day", "past_week", "past_month",
"past_year"], "type": "string"}, "query": {"description": "Search query in Google search style, using 3-5 keywords.", "type":
"string"}}, "required": ["query"], "type": "object"}}

{"description": "View content of the current browser page.\n\nRecommended scenarios:\n- When checking the latest state of
previously opened pages\n- When monitoring progress of operations (e.g., progress bars)\n- When saving screenshots of pages
in specific states\n- Before using other tools that require element index numbers\n\nBest practices:\n- Page content is
automatically provided after navigation to a URL, no need to use this tool specifically\n- This tool is primarily for
checking the updated state of previously opened pages after some time\n- Can be used repeatedly to wait and monitor
completion status of operations in web applications\n- When opening files like PDFs, use this tool to wait for complete
loading if blank content is encountered", "name": "browser_view", "parameters": {"type": "object"}}

{"description": "Navigate browser to specified URL.\n\nRecommended scenarios:\n- When search results list is obtained from
search tools\n- When URLs are provided in user messages\n- When accessing new pages is needed\n- When refreshing current
page\n\nBest practices:\n- Ensure URL format is correct and complete\n- Check page response status", "name":
"browser_navigate", "parameters": {"properties": {"url": {"description": "Complete URL to visit. Must include protocol prefix
(e.g., https:// or file://).", "type": "string"}}, "required": ["url"], "type": "object"}}

{"description": "Click on elements in the current browser page.\n\nRecommended scenarios:\n- When clicking page elements is
needed\n- When triggering page interactions\n- When submitting forms\n\nBest practices:\n- Ensure target element is visible
and clickable\n- Must provide either element index or coordinates\n- Prefer using element index over coordinates", "name":
"browser_click", "parameters": {"properties": {"coordinate_x": {"description": "(Optional) Horizontal coordinate of click
position, relative to the left edge of the current viewport.", "type": "number"}, "coordinate_y": {"description": "(Optional)
Vertical coordinate of click position, relative to the top edge of the current viewport.", "type": "number"}, "index":
{"description": "(Optional) Index number of the element to click", "type": "integer"}}, "type": "object"}}

{"description": "Overwrite text in editable elements on the current browser page.\n\nRecommended scenarios:\n- When filling
content in input fields\n- When updating form fields\n\nBest practices:\n- This tool first clears existing text in target
element, then inputs new text\n- Ensure target element is editable\n- Must provide either element index or coordinates\n-
Prefer using element index over coordinates\n- Decide whether to press Enter key based on needs", "name": "browser_input",
"parameters": {"properties": {"coordinate_x": {"description": "(Optional) Horizontal coordinate of the element to overwrite
text, relative to the left edge of the current viewport.", "type": "number"}, "coordinate_y": {"description": "(Optional)
Vertical coordinate of the element to overwrite text, relative to the top edge of the current viewport.", "type": "number"},
"index": {"description": "(Optional) Index number of the element to overwrite text", "type": "integer"}, "press_enter":
{"description": "Whether to press Enter key after input", "type": "boolean"}, "text": {"description": "Complete text content
to overwrite", "type": "string"}}, "required": ["text", "press_enter"], "type": "object"}}

{"description": "Move cursor to specified position on the current browser page.\n\nRecommended scenarios:\n- When simulating
user mouse movement\n- When triggering hover effects\n- When testing page interactions\n\nBest practices:\n- For clicking,
use browser_click tool directly without moving cursor first", "name": "browser_move_mouse", "parameters": {"properties":
{"coordinate_x": {"description": "Horizontal coordinate of target cursor position, relative to the left edge of the current
viewport.", "type": "number"}, "coordinate_y": {"description": "Vertical coordinate of target cursor position, relative to
the top edge of the current viewport.", "type": "number"}}, "required": ["coordinate_x", "coordinate_y"], "type": "object"}}
{"description": "Simulate key press in the current browser page.\n\nRecommended scenarios:\n- When specific keyboard
operations are needed\n- When keyboard shortcuts need to be triggered\n\nBest practices:\n- Use standard key names\n- Use
plus sign to connect combination keys", "name": "browser_press_key", "parameters": {"properties": {"key": {"description":
"Key name to simulate (e.g., Enter, Tab, ArrowUp), supports key combinations (e.g., Control+Enter).", "type": "string"}},
"required": ["key"], "type": "object"}}

{"description": "Select specified option from dropdown list element in the current browser page.\n\nRecommended scenarios:\n-
When selecting dropdown menu options\n- When setting form select fields\n\nBest practices:\n- Ensure dropdown list is
interactive", "name": "browser_select_option", "parameters": {"properties": {"index": {"description": "Index number of the
dropdown list element", "type": "integer"}, "option": {"description": "Option number to select, starting from 0.", "type":
"integer"}}, "required": ["index", "option"], "type": "object"}}

{"description": "Scroll up the current browser page.\n\nRecommended scenarios:\n- When viewing content above\n- When
returning to page top\n- When preparing to interact with elements above\n\nBest practices:\n- Defaults to scroll up one
viewport; use `to_top` parameter to scroll directly to page top\n- Multiple scrolls may be needed to gather enough
information", "name": "browser_scroll_up", "parameters": {"properties": {"to_top": {"description": "(Optional) Whether to
scroll directly to page top instead of one viewport up, defaults to false.", "type": "boolean"}}, "type": "object"}}
{"description": "Scroll down the current browser page.\n\nRecommended scenarios:\n- When viewing content below\n- When
jumping to page bottom\n- When preparing to interact with elements below\n- When triggering lazy-loaded content\n\nBest
practices:\n- Defaults to scroll down one viewport; use `to_bottom` parameter to scroll directly to page bottom\n- Must use
scrolling instead of relying on extracted markdown content when page contains many visual elements like images\n- Must use
scrolling to view content when page markdown is not fully extracted\n- Multiple scrolls may be needed to gather enough
information\n- Pay attention to dynamically loaded content triggered by scrolling", "name": "browser_scroll_down",
"parameters": {"properties": {"to_bottom": {"description": "(Optional) Whether to scroll directly to page bottom instead of
one viewport down, defaults to false.", "type": "boolean"}}, "type": "object"}}

{"description": "Execute JavaScript code in browser console.\n\nRecommended scenarios:\n- When custom scripts need to be
executed\n- When page element data needs to be retrieved\n- When debugging page functionality or manipulating DOM\n\nBest
practices:\n- Ensure code is safe and controlled\n- Wait for asynchronous operations when necessary", "name":
"browser_console_exec", "parameters": {"properties": {"javascript": {"description": "JavaScript code to execute. Note that
the runtime environment is browser console.", "type": "string"}}, "required": ["javascript"], "type": "object"}}
{"description": "View browser console output.\n\nRecommended scenarios:\n- When checking JavaScript logs\n- When debugging
page errors\n- When verifying script execution results\n\nBest practices:\n- Set reasonable line limit", "name":
"browser_console_view", "parameters": {"properties": {"max_lines": {"description": "(Optional) Maximum number of log lines to
return, defaults to last 100 lines.", "type": "integer"}}, "type": "object"}}

{"description": "Save image from current browser page to local file.\n\nRecommended scenarios:\n- When downloading images
from web pages\n- When collecting assets for creating web pages or documents\n\nBest practices:\n- Coordinates can be any
point within the image element, center point recommended\n- Set save directory to corresponding working directory when saving
images as assets to avoid extra copying\n- Base name should be semantic and human-readable, avoid special characters or
spaces\n- Extension will be added automatically based on image format, no need to include in base name\n- Final save path is
determined by save_dir, base_name, and image format, will be returned in result", "name": "browser_save_image", "parameters":
{"properties": {"base_name": {"description": "Base name (stem) for the image file, without directory or extension. e.g.,
\"apollo_11_landing_site\", \"albert_einstein_portrait\".", "type": "string"}, "coordinate_x": {"description": "Horizontal
coordinate of the image element to save, relative to the left edge of the current viewport.", "type": "number"},
"coordinate_y": {"description": "Vertical coordinate of the image element to save, relative to the top edge of the current
viewport.", "type": "number"}, "save_dir": {"description": "Local directory to save the image file (must use absolute path)",
"type": "string"}}, "required": ["coordinate_x", "coordinate_y", "save_dir", "base_name"], "type": "object"}}

{"description": "Expose specified local port for temporary public access.\n\nRecommended scenarios:\n- When providing
temporary public access for services that cannot be deployed in production\n\nBest practices:\n- This tool returns a
temporary public proxied domain for the specified port\n- Port information is encoded in domain prefix, no additional port
specification needed\n- Confirm service is running and tested locally before using this tool\n- Exposed services should not
bind to specific IP addresses or Host headers", "name": "deploy_expose_port", "parameters": {"properties": {"port":
{"description": "Local port number to expose", "type": "integer"}}, "required": ["port"], "type": "object"}}

{"description": "Deploy website or application to public production environment.\n\nRecommended scenarios:\n- When deploying
or updating static websites\n- When deploying or updating Next.js applications\n\nBest practices:\n- This tool returns a
permanent public URL after successful deployment\n- Static website directory must be a built static files directory (e.g.
/path/to/dist/ or /path/to/build/)\n- Next.js application directory must be the project root directory created by template
commands\n- Websites or applications must be tested locally and confirmed by users before deployment\n- Use this tool
repeatedly to update the deployed websites or applications\n- Websites or applications should not bind to specific IP
addresses or Host headers", "name": "deploy_apply_deployment", "parameters": {"properties": {"local_dir": {"description":
"Absolute path of local directory to deploy.\n- For static websites, directory must contain index.html file\n- For Next.js
applications, directory must be project root directory", "type": "string"}, "type": {"description": "Type of website or
application to deploy.\n- static: Static website\n- nextjs: Next.js application", "enum": ["static", "nextjs"], "type":
"string"}}, "required": ["type", "local_dir"], "type": "object"}}
```
### Function Call Format
When invoking functions, the following format is used:
```
<function_calls>
<invoke name="$FUNCTION_NAME">
<parameter name="$PARAMETER_NAME">$PARAMETER_VALUE
