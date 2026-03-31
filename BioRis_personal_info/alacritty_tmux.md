# Prefix has been changed to ctrl+s


## show history
- history = show history newest to oldest
- history | cat = show history newest to oldest
- history | head = show first 10 commands
- history | tail = show last 10 commands
- history | grep [pattern] = search for a pattern
- history | sort = sort the history
- history | uniq = remove duplicate history

## sessions & tabs
1. tmux new -s myname
2. tmux attach -t myname
3. rename session: prefix then $ then name
4. rename tab: prefix then ,
5. show sessions: prefix then s
6. kill session: tmux kill-session -t sessionNumber
7. kill all sessions: tmux kill-session -a
8. split horizontally: prefix then "
9. split vertically: prefix then %
10. List sessions: tmux ls
11. 

## Tabs
1. new tab: prefix then c
2. show tabs: prefix then s
3. rename tab: prefix then ,
4. close tab: prefix then x
5. switch to tab number: prefix then [number]

## Panes
1. split horizontally: prefix then -
2. split vertically: prefix then |
3. switch to next pane: prefix then o
4. switch to pane number: prefix then [number]
5. close pane: prefix then x
6. resize pane: prefix then :
7. move between panes: prefix then arrow keys
8. zoom pane: prefix then z
9. unzoom pane: prefix then z

# Alacritty + Tmux Commands
## Most Used
1. new tab: prefix then c
2. close tab: prefix then x
3. split horizontally: prefix then "
4. split vertically: prefix then %
5. show tabs: prefix then s
6. switch to tab number: prefix then [number]
7. move between panes: prefix then arrow keys
8. scroll mode: prefix then [
9. detach session: prefix then d
10. reattach session: tmux attach
11. Start a new session: tmux new -s myname
12. List sessions: tmux ls
13. Attach to a named session: tmux attach -t myname
16. Kill a session: tmux kill-session -t myname

## Oh My Tmux / Advanced
8. open config: prefix then e
9. reload config: prefix then r
10. clear screen and history: ctrl+l
11. new session: prefix then ctrl+c
12. find/switch session: prefix then ctrl+f
13. switch to last session: prefix then shift+tab
14. split pane vertically: prefix then -
15. split pane horizontally: prefix then _
16. move between panes (vi): prefix then h/j/k/l
17. swap pane down/up: prefix then > or <
18. maximize/restore pane: prefix then +
19. resize pane: prefix then H/J/K/L
20. previous/next window: prefix then ctrl+h or ctrl+l
21. last active window: prefix then tab
22. toggle mouse mode: prefix then m
23. urlview: prefix then U
24. facebook pathpicker: prefix then F
25. enter copy mode: prefix then enter
26. copy-mode (vi) keys: v, ctrl+v, y, escape, H, L
27. copy to system clipboard: prefix then y
28. list/paste/choose buffers: prefix then b/p/P




