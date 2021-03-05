# How to debug using Delve inside NeoVim

This is a simple explanation on how to use [delve](https://github.com/go-delve/delve) inside NeoVim using [vim-delve](https://github.com/sebdah/vim-delve).

On NeoVim, add a breakpoint with `:DlvAddBreakpoint` and run `:DlvDebug` or `:DlvTest`.

To run a single test one can use `:DlvTest -- -test.run RegexpWithTestName`

Then run Delve commands as normal.

Some references:

- [vim-delve commands](https://github.com/sebdah/vim-delve#commands)
- [general delve commands](https://github.com/go-delve/delve/blob/master/Documentation/cli/README.md)

# How to run a single test

At your terminal use `go test -run RegexpWithTestName <path where the tests are>`. Example:

```
go test -v -run Test.\*Iam ./aws
```
