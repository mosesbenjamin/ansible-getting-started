# ansible-getting-started

__Adhoc configuration with indempotent modules__
- Create a ".sh" file
- Make the script executable: chmod +x filename.sh
- Now run the script with "./filename.sh" to repeat configuration
- "git config --add" is not indempotent because it duplicates config
- Shell scripts are imperative (concerned with how a desired stage is reached) whereas ansible is declarative (What desired state should look like)
- Reconciliation is the difference between current and desired state, and the what's necessary to get from the former to the latter.
- Procedural / imperative is partially automated with commands; Declarative is fully automated. 