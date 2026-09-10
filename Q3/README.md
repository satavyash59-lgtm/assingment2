bugs

1. stage (checkout) should be present inside the stages
2. also stage (checkout) is opened not closed
3. Parallel stages must be properly enclosed within a valid parent stage and `parallel { ... }` block
4. mains stages block is not closed
5. insted of this `sh 'rm -rf workspace/*'` use built in cleanWs().
6. final pipe line curly brases are missing.
