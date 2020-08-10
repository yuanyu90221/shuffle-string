# shuffle-string

## 題目解讀：

### 題目來源:

[shuffle-string](https://leetcode.com/problems/shuffle-string/)
### 原文:
Given a string s and an integer array indices of the same length.

The string s will be shuffled such that the character at the ith position moves to indices[i] in the shuffled string.

Return the shuffled string.

### 解讀：
給定一個 字串 s 還有一個 正整數陣列 indice 跟s 的長度相同

其中 每個 indice 中的 元素 代表 s對應位置的字元的順序

舉例來說 ： s = "acb" ,indice = [0, 2, 1]

則 restoreString 變成 "abc" 因為 c 對應的位置為2 


## 初步解法:
### 初步觀察:

自己當初的作法：

因為array本身紀錄著位置

因此可以透過

建立 map[array[i]] = s[i]

接著就可以透過每個index的值來把原本的string還原

後來看別人的作法：

其實不需要特別建立額外的map來存儲

因為直接建立一個character array result = make([]rune, len(s))

然後 剛好有以下這個關係 result[indice[i]] = s[i]

### 初步設計:
原本自己的想法：
given a string s, and an integer array indice

step1: let a string resultStr = ""

step2: create a map valueMap = make(map[int]rune)

step3: loop idx = 0 to idx < length of s, valueMap[indice[idx]] = s[idx]

step4: loop idx = 0 to idx < length of s, resultStr += string(valueMap[idx])

step5: return resultStr
後來看到別人的作法：
given a string s, and an integer array indice

step1: let a string resultStr = ""

step2: create a character array result = make([]rune, len(s))

step3: loop idx = 0 to idx < length of s, result[indice[idx]] = s[idx]

step4: return string(result)
## 遇到的困難
### 需要知道golang rune 與string的關係
首先需要知道

character array 可以透過 string constructor轉換成string
### 題目上理解的問題
因為英文不是筆者母語

所以在題意解讀上 容易被英文用詞解讀給搞模糊

### pseudo code撰寫

一開始不習慣把pseudo code寫下來

因此 不太容易把自己的code做解析

### golang table driven test不熟
對於table driven test還不太熟析

所以對於寫test還是耗費不少時間
## 我的github source code
原本自己的作法
```golang
package restore_string

func restoreString(s string, indices []int) string {
	result := ""
	valueMap := make(map[int]rune)
	for idx, val := range s {
		valueMap[indices[idx]] = val
	}
	for idx, _ := range s {
		result += string(valueMap[idx])
	}
	return result
}

```
最後參考別人的作法：
```golang
package restore_string

func restoreString(s string, indices []int) string {
	result := make([]rune, len(s))
	for idx, val := range s {
		result[indices[idx]] = val
	}
	return string(result)
}

```

## 測資的撰寫
```golang
package restore_string

import "testing"

func Test_restoreString(t *testing.T) {
	type args struct {
		s       string
		indices []int
	}
	tests := []struct {
		name string
		args args
		want string
	}{
		{
			name: "Example1",
			args: args{
				s:       "codeleet",
				indices: []int{4, 5, 6, 7, 0, 2, 1, 3},
			},
			want: "leetcode",
		},
		{
			name: "Example2",
			args: args{
				s:       "abc",
				indices: []int{0, 1, 2},
			},
			want: "abc",
		},
		{
			name: "Example3",
			args: args{
				s:       "aiohn",
				indices: []int{3, 1, 4, 2, 0},
			},
			want: "nihao",
		},
		{
			name: "Example4",
			args: args{
				s:       "aaiougrt",
				indices: []int{4, 0, 2, 6, 7, 3, 1, 5},
			},
			want: "arigatou",
		},
		{
			name: "Example5",
			args: args{
				s:       "art",
				indices: []int{1, 0, 2},
			},
			want: "rat",
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			if got := restoreString(tt.args.s, tt.args.indices); got != tt.want {
				t.Errorf("restoreString() = %v, want %v", got, tt.want)
			}
		})
	}
}

```

## 參考文章

[golang test](https://ithelp.ithome.com.tw/articles/10204692)