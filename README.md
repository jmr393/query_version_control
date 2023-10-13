This Function will use the Metabase and Gitlab APIs to implement version control for marked queries. The function uses DynamoDB to save the state of current PRs, unresolved changes, and when to revert them.

Steps:
Search queries for new tags => if new, create PR creating file, add dynamodb item (id, file)
Search dynamodb/repo and metabase for disagreements

9 Different Cases:
- Case 1: No open PR and no .sql file on the main branch:
  - Create a new PR to add the Metabase query to the repo as a new file
- Case 2: No open PR and the .sql file on the main branch does not match the current Metabase query
  - Create a new PR to update the file
- Case 3: No open PR and the .sql file on the main branch matches the current Metabase query
  - No action
- Case 4: There is an open PR, but the current query on Metabase matches the .sql file on main
  - Close the PR without merging
- Case 5: There is an open PR, but the PR has been open for more than a week
  - Close the PR without merging
- Case 6: The open PR matches neither the .sql file on the main branch nor the new branch
  - Update the PR to reflect the new query in Metabase
- Case 7: The current Metabase query matches the .sql file in the open PR
  - No action
- Case 8: The current Metabase query is not on main and does not match the .sql file in the new branch
  - Update the PR to reflect the new query in Metabase
- The current Metabase query is not on main and it matches the .sql file in the new branch
  - No action

![alt text](logic_flowchart.png)
