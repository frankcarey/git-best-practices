The Opinionated Git Best Practices
==================================

I've collected some best practices while using git for the years and I'd like to share them in a way that other teams and individuals can leverage them as wholistic vision of how to use git "the right way". If you find a better pattern of usage, feel free to fork it and update it, even submit a pull request so we might update this over time.

Aren't there already books on git and documentation?
---------------------------------
Yes there is some great documentation for git out there, but the focus is often around the commands themselves, and not a clearer guide on the best ways to use it. In addition, there are things like "Pull Requests" that are not even officially part of git, but that are ubiquitous concepts now thanks to github, bitbucket, etc. It's not always clear the best ways teams might use git as a whole, and what works and what doesn't work well when in those situations.

First, you need to pick a workflow
----------------------------------

Atlassian has a good overview of the (various flavors of git workflows)[https://www.atlassian.com/git/tutorials/comparing-workflows] out there, and we'll use their terminology for simplicity. It's a clean description, and I would recommend at least reading the summaries.

Git makes it easy to use almost the same workflows no matter which you choose, so we're going to add some details left out of the Atlassian doc about when to use which type.

We're going to assume that you are using git, but also something like Github for pull requests.

### Centralized Workflow AKA free-for-all
Here, everyone works on their stuff in master or at least pushes to master when it's ready. This is VERY rarely a good idea unless it's just you coding something up all on your own, or hacking with a small team at a hackathon. Moving on..

### Feature / Dev Branches
This keeps one central repo that everyone has access to, but instead of pushing to master, team members create a branch to keep their work in, and push that branch up to the server instead of master. This is usually immediately followed by a "Pull Request" where a tool like github allows you review the code, leave comments, kick-off automated testing, etc. People often don't merge their own pull requests, but ask for other devs to review it first.. more on that later.

### Gitflow Workflow - Multiple branches for development (1.x and 2.x)
For bigger projects with rolling releases or multiple versions that need to be supported and updated, this is a good workflow. Again you should read the article for details, but it basically is a way to have major and minor versions of your project and change them at the same time. You can always add this as you need it.

### Forking Workflow - Everyone has their own repos on the server.
This is basically how public github repos work. You fork another person's repo then make changes in a dev branch. If you want to commit it back "upstream", you push your change up to your server-side repo (since you don't have push access to the other person's repo) and submit a Pull Request. This is what made github.com a hige site. Anyone can easily make a copy of the code, then submit a pull request when they've added or fixed something. Similarly, this can be good for really large teams, where you want to enforce workflows like to ensure that no-one can push to master directly, when you have outside developers or new developers that might screw things up, when you want to open source your code and get help from an outside community.

WINNER: Dev branches + One server side repo
--------------------

If you have a smallish team of around 10 in-house developers, start there. You can always layer on special release branches, and more process if things get chaotic. You can even have a hibrid where most of the in-house team can access the main repo, but perhaps contractors or new employees start with their own fork until the get the hang of it. You enforce things through the honor system that people will not push to master directly, but use pull requests.

The downside of "everyone has their own fork" is that things can get a little complex when you want to grab someone elses branch, and people end up having a LOT of remotes that they need to setup. There is also more setup to create the upstream (canonical) repo so you can pull changes. We'll cover some best practices if you gradutate to this level of complexity, but I wound't recommend it as your default. Again, you can always add it later.
