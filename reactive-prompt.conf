# Reactive Prompt functions
#
# Forked mercilessly from https://github.com/codeslinger
#
# Example:
#   user@hostname ~/current/working/directory gitbranch ++
#   [1] >: your-command-here
#
# Features:
#   - if error, exit code in red "[1]" next to prompt
#   - current git working branch
#   - appends purple "++" if current branch has outstanding changes
#   - Lost-style prompt: ">:"

[ -n "$PS1" ] && bind "set completion-ignore-case on"

# Returns 0 (success) if the pwd is tracked, otherwise 1 (failure).
git_pwd_is_tracked() {
   [ $(git log -1 --pretty=oneline . 2> /dev/null | wc -l) -eq "1" ]
}

# Emits the current time in 24-hr notation.
show_time() {
  echo "${COLOR_GRAY}$(date +%H:%M)${COLOR_NONE}"
}

# Stores the exit status of the last command for use by show_exit_status function.
if [[ ! $PROMPT_COMMAND =~ store_exit_status ]]; then
  export PROMPT_COMMAND="store_exit_status && ${PROMPT_COMMAND:-:}"
fi
store_exit_status() {
  LAST_EXIT_STATUS=$?
}

# Emits exit status of last command if error condition returned.
show_exit_status() {
  if [ "x${LAST_EXIT_STATUS}" != "x0" ]; then
    echo "${LAST_EXIT_STATUS} "
  fi
}

# Emits the current git branch and marker if there are outstanding changes.
show_git_branch_and_status() {
  if git_pwd_is_tracked; then
    local branch_prompt
    branch_prompt=$(__git_ps1 "%s")
    if [[ -n "$branch_prompt" ]]; then
      echo " ${COLOR_GRAY}[${COLOR_NONE}${COLOR_YELLOW}$branch_prompt${COLOR_NONE}$(show_git_status)${COLOR_GRAY}]${COLOR_NONE}"
    fi
  fi
}

# Removes display of git status from prompt, useful for large repositories.
hide_git_status() {
  show_git_branch_and_status() { exit; }
}

# Emits a purple "++" if current repository is 'dirty'.
show_git_status() {
  [[ $(git status 2> /dev/null | tail -n1) != "nothing to commit (working directory clean)" ]] && echo " ${COLOR_LIGHT_RED}★ ${COLOR_NONE}"
} 

prompt_color(){
  echo "${COLOR_LIGHT_GRAY}"
}