### Configuring Git
- Config settings can be specified at 3 levels
	- **system:** all users
	- **global:** all repos of current user
	- **local:** current repo only
- Setting default editor: `git config --glocal core.editor "cursor --wait"`
	- i'm using `cursor` here instead of `code` coz I use cursor instead of vscode
	- `--wait` flag will put the terminal on pause (can't proceed with new commands) until you have closed the editor
- Editing the global settings using IDE: `git config --global -e`
	- opens `.gitconfig` 
- **Handling end-of-line:** `git config --global core.autocrlf input`
	- in windows, eol is denoted by `\r\n`, while in mac, it's just `\n`
	- in windows, instead of `input`, set it to `true`
### Git Workflow
- Git does not store delta among files, rather it stores the entire file
- But, it does this smartly by compressing the files and not storing duplicate contents
	- requires understanding of implementation details (out of scope)
### Staging Files
- 
### Cheatsheet
<div style="overflow: hidden; padding-top: 56.25%; position: relative;">
  <iframe src="Git Cheatsheet Mosh.pdf" style="border: 0; top: 0; left: 0; width: 100%; height: 100%; position: absolute;"></iframe>
</div>

---
## References

1. **Programming with Mosh**, *Git Tutorial for Beginners: Learn Git in 1 Hour*: https://www.youtube.com/watch?v=8JJ101D3knE