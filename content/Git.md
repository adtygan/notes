### Configuring Git
- Config settings can be specified at 3 levels
	- **System:** All users
	- **Global:** All repos of current user
	- **Local:** Current repo only
- Setting default editor: `git config --glocal core.editor "cursor --wait"`
	- I'm using `cursor` here instead of `code` coz I use Cursor instead of VSCode
	- `--wait` flag will put the terminal on pause (can't proceed with new commands) until you have closed the editor
- Editing the global settings using IDE: `git config --global -e`
	- Opens `.gitconfig` 
- **Handling end-of-line:** `git config --global core.autocrlf input`
	- In Windows, EOL is denoted by `\r\n`, while in Mac, it's just `\n`
	- In Windows, instead of `input`, set it to `true`
### Git Workflow
- Git does not store delta among files, rather it stores the entire file
- But, it does this smartly by compressing the files and not storing duplicate contents
	- Requires understanding of implementation details (out of scope)
### Staging Files
- 
### Cheatsheet
<div style="overflow: hidden; padding-top: 56.25%; position: relative;">
  <iframe src="Git Cheatsheet Mosh.pdf" style="border: 0; top: 0; left: 0; width: 100%; height: 100%; position: absolute;"></iframe>
</div>

---
## References

1. **Programming with Mosh**, *Git Tutorial for Beginners: Learn Git in 1 Hour*: https://www.youtube.com/watch?v=8JJ101D3knE