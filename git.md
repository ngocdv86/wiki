<div id="top"></div>

<br />
<div align="center">
  <h1 align="center">Git</h1>
</div>

## Common command

- Merge commit

  ```sh
  git reset --mixed commit_id
  git add .
  git commit -m'commit messages'
  git push -f
  ```

  Merge all commit after `commit_id` (not include `commit_id`) to new commit.

  Example:

  ```sh
  git reset --mixed d46bc32f00bdff96093656255da37e6da5b44392
  git add .
  git commit -m'commit messages'
  git push -f
  ```

  <p align="right">(<a href="#top">Back to top</a>)</p>
