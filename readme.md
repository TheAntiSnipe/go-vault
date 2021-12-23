P### This will be a quick guide into how to get this repository working, and how the workflow for it will look like.

#### Starting from scratch

First thing you need to do is press Fork, and fork my repo to your end.
The processes that follow, assume that you have SSH set up. If you don't have it set up, follow [this source for it](https://kbroman.org/github_tutorial/pages/first_time.html)

*Step 1:* Navigate to your folder of choice, and `git clone your-ssh-link-copied-from-clipboard`
	![](https://i.imgur.com/rGe7TkU.png)


^^ Copy from here (this is an example btw)

*Step 2:* `cd go-vault`
*Step 3:* `git remote add upstream https://github.com/TheAntiSnipe/go-vault.git`

This concludes the setup!

---
#### Working on changes and pushing them to the main vault

Okay, so you've got changes that you've made, and you want to send them up to the main vault so everyone can see them.

*Step 1:* Navigate to your vault
*Step 2*: (Optional) If there's been changes to the upstream repo, `git pull upstream main`
*Step 2:* `git add .`
*Step 3:* `git commit -m "(Name of person) : (A short description of changes)"`
*Step 4:* `git push -u origin main`

At step 4, your changes are up on your local fork, on GitHub. Open your browser, open GitHub to your fork. You'll see something like this:
	![](https://i.imgur.com/RJFKec5.png)
^^ Again, remember that this is just an example. Anyway, click on Contribute, Open Pull Request, and merge your pull request. If you're a contributor I know and have added, you can merge it yourself. If you aren't, one of the collaborators will push it for you later.

And that's about it! I'm not very good at version control myself, so if anyone here has changes and best practices they want me to add to this list, let me know!

Anti out!

---
