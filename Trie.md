#字典树的创建和应用
##1.字典树的创建
字典树也称为前缀树，应用于单词查询，代码补全等。每个节点可以有多个子节点，节点与节点之间的路表示一个字母。当节点标记为结束节点时表示一个单词的查询到此为止。

根节点创建：


	public class TrieNode {
		int count;  
		int prefix;  
		//treenode数组表示他的子节点们，以对应字母在1-26中对应
		的数字为下标。如添加一个c为路径的子节点，标记下标
		为'c'-'a'，也就是2。
		TrieNode[] nextNode=new TrieNode[26];  
		public TrieNode(){
			count=0;
			prefix=0;
		}
	}

count表示以此节点结尾的单词数；  

prefix表示此节点之前的单词作为前缀的数量，简而言之就是有几个count不为0的分支就有几个prefix

##2.字典树的查询操作  

遍历字典，创建分支：

	public static void insert(TrieNode root,String str){
		if(root==null||str.length()==0){
			return;
		}
		char[] c=str.toCharArray();
		for(int i=0;i<str.length();i++){
			//如果该分支不存在，创建一个新节点
			if(root.nextNode[c[i]-'a']==null){
				root.nextNode[c[i]-'a']=new TrieNode();
			}
			root=root.nextNode[c[i]-'a'];
			root.prefix++;//出现新分支了，作为前缀的数量加一
			}
		
		//以该节点结尾的单词数+1
		root.count++;
	}


查询操作：

	public static int search(TrieNode root,String str){
		if(root==null||str.length()==0){
			return -1;
		}
		char[] c=str.toCharArray();
		for(int i=0;i<str.length();i++){
			//如果该分支不存在，表名该单词不存在
			if(root.nextNode[c[i]-'a']==null){
				return -1;
			}
			//如果存在，则继续向下遍历
			root=root.nextNode[c[i]-'a'];	
		}
		
		//如果count==0,也说明该单词不存在

		可能字典树中有对应分支，但是不在字典中，count=0
		if(root.count==0){
			return -1;
		}
		return root.count;
	}
 




它的优点是：最大限度地减少无谓的字符串比较。

Trie的核心思想是空间换时间。利用字符串的公共前缀来降低查询时间的开销以达到提高效率的目的。