---
title: Mirroring my github repos to bitbucket
date: 2022-04-15
tags:
	- solution
---

# Mirroring my github repos to bitbucket #

## Update: ##
You might wanna take a look at `git ... --mirror`

On 13th of April, 2022 I was stuck with a ref lock thing with my portfolio repo. It was really dirty one and the only way I solved it was importing the repo to bitbucket & reimporting it back. Made me think about backups. 

Fortunately, I was tinkering with Ansible[^1]. It was a good learning opporunity, mirror my repos using Ansible and I did it.

Pretty sure this isn't the best way to achieve this but it works.

Here's the code, Its cronified for every hour on my vps.

```yaml
- name: Mirror github to bitbucket
  # hosts: gcp
  hosts: localhost
  connection: local
  
  vars:
    git_username: "jarusll"
    git_dir: "~/source/ansible_clones"
    prefix:
      github: "git@github.com"
      bitbucket: "git@bitbucket.org"
    remote:
      github: "origin"
      bitbucket: "bit"
    repos:
      - jarusll.github.io
      - diary
      - posts
      - fragments
      - artworks
      - dotfiles
      
  tasks:
    - name: Clone all repos from github
      ansible.builtin.git:
        repo: "{{prefix.github}}:{{git_username}}/{{item}}.git"
        clone: yes
        dest: "{{git_dir}}/{{item}}"
        update: yes
      register: repo
      loop: "{{repos}}"

    - name: Add bitbucket remotes to all repos
      ansible.builtin.shell:
        cmd: "git remote add {{remote.bitbucket}} {{prefix.bitbucket}}:{{git_username}}/{{item}}.git"
        chdir: "{{git_dir}}/{{item}}"
      loop: "{{repos}}"
      ignore_errors: yes

    - name: Push to bitbucket
      ansible.builtin.shell:
        cmd: "git push bit"
        chdir: "{{git_dir}}/{{item}}"
      loop: "{{repos}}"
      ignore_errors: yes

```

Check out my [Ansible playground repo](https://github.com/jarusll/ansible-playground)

[^1]: https://www.ansible.com
