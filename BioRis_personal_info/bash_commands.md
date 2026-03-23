# Bash commands
## Navigation
1. cd [directory] = change directory
2. cd - = go back to previous directory
3. cd .. = go back one directory
4. cd ~ = go to home directory
5. pwd = print working directory
6. ls = list files
7. ls -l = list files in long format
7. ls -a = list all files
8. ls -la = list all files in long format
9. tree = list files in tree format

## File Managment
10. cp -r [source] [destination] = copy files recursively (folders)
11. mv file newname = rename or move file
12. rm -rf folder/ = remove folders (careful!)
13. mkdir -p a/b/c/d/e = create nested directories
14. touch file.txt = create empty file
15. find . -name "*.txt" = find files by name
16. find . -name "*.txt" | wc -l = count files
17. find . -type d -name "*.txt" = find directories by name
18. find . -type d -name "*.txt" | wc -l = count directories
19. locate [file] = find files by name (faster than find)
20. ln -s target link = create a symbolic link

## File manipulation
21. cat [file] = concatenate files
22. cat [file] | head = show first 10 lines
23. cat [file] | tail = show last 10 lines
24. cat [file] | grep [pattern] = search for a pattern
25. cat [file] | wc = count lines, words, and characters
26. cat [file] | sort = sort the file
27. cat [file] | uniq = remove duplicate lines
28. cat [file] | diff = compare two files
29. cat [file] | sed = stream editor
30. cat [file] | awk = pattern scanning and processing language

# Regla mental rápida (muy útil)

- Si estás en terminal y dudas:

- extraer columna → cut

- reemplazar texto → sed

- filtrar o analizar → awk

## block overwriting
31. set -o noclobber = block overwriting
32. set +o noclobber = unblock overwriting
 33. echo hi > file.txt = bash blocks this action
 to do it then when sure do:
    34. echo hi >| file.txt