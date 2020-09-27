We achieve a huge performance boost on slicing and concatenation operations using bytes as opposed to strings.
As can be seen in the below performance results, it takes ~60s to execute the benchmark test suite using strings and ~19s using byte arrays to store blocks of text.
 

Test- Using string

goos: windows
goarch: amd64
BenchmarkClipboard/CutPaste170-12               15624918                76.1 ns/op
BenchmarkClipboard/CopyPaste170-12               1000000             25543 ns/op
BenchmarkClipboard/GetText170-12                586315111                1.90 ns/op
BenchmarkClipboard/Misspellings170-12            2061801               579 ns/op
BenchmarkClipboard/CutPaste1700-12               4106518               307 ns/op
BenchmarkClipboard/CopyPaste1700-12              1000000             27288 ns/op
BenchmarkClipboard/GetText1700-12               572749987                2.01 ns/op
BenchmarkClipboard/Misspellings1700-12            240148              5124 ns/op
PASS
ok      command-line-arguments  61.593s

Test- Using []byte

goos: windows
goarch: amd64
BenchmarkClipboard/CutPaste170-12               75206347                15.4 ns/op
BenchmarkClipboard/CopyPaste170-12               1000000              5501 ns/op
BenchmarkClipboard/GetText170-12                20735594                49.9 ns/op
BenchmarkClipboard/Misspellings170-12            1889653               641 ns/op
BenchmarkClipboard/CutPaste1700-12              50129919                24.6 ns/op
BenchmarkClipboard/CopyPaste1700-12              1000000              5399 ns/op
BenchmarkClipboard/GetText1700-12                4718612               259 ns/op
BenchmarkClipboard/Misspellings1700-12            221607              5389 ns/op
PASS
ok      command-line-arguments  19.199s

A search operation is added to the existing operations of the editor.
This operation returns the start and end indices of all the given search query's occurrences in the document.
The search operation is faster on strings as compared to bytes which is why the document is converted to a string before searching for occurrences. 


goos: windows
goarch: amd64
BenchmarkClipboard/CutPaste170-12               92556883                12.8 ns/op
BenchmarkClipboard/CopyPaste170-12               1000000              5624 ns/op
BenchmarkClipboard/GetText170-12                19664715                53.0 ns/op
BenchmarkClipboard/Misspellings170-12            1899789               637 ns/op
BenchmarkClipboard/Search170-12                   333982              3359 ns/op
BenchmarkClipboard/CutPaste1700-12              50051510                25.0 ns/op
BenchmarkClipboard/CopyPaste1700-12              1000000              5693 ns/op
BenchmarkClipboard/GetText1700-12                4457662               269 ns/op
BenchmarkClipboard/Misspellings1700-12            211094              5604 ns/op
BenchmarkClipboard/Search1700-12                   54920             21227 ns/op
PASS
ok      command-line-arguments  22.176s


The above changes to the basic data structure were done because strings are faster for searches (contains, index, compare) whereas bytes are faster in creation operations (replace, concat)


To run the program:
execute the following command
go test -bench=. editor_test.go