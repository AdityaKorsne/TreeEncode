import java.util.*;

public class Tree1 {
	
	Tree1 left;
	Tree1 right;
	
	public String toString()
  {
        return "Tree1 { left = " + left +" , right = " + right +" }";
  }
	
	public static void main(String[] args) {
		TreeSet<Character> ts=new TreeSet<Character>();
		ts.add('}');
		ts.add(')');
		ts.add(']');
		System.out.println(ts);
	}
}

public class Leaf1 extends Tree1 {

	public String toString()
  {
        return "Leaf1 { left = " + left +" , right = " + right +" }";
  }
}

public class Branch1 extends Tree1 {
	
	Tree1 left;
	Tree1 right;
	public Branch1(Tree1 left,Tree1 right)
	{
		this.left=left;
		this.right=right;
	}
	
	public String toString() 
  {
        return "Branch1 { left = " + left +" , right = " + right +" }";
  }
}

public class EncodeDecode {
		
	public static String treeToParens(Tree1 tree) 
	{
        if (tree instanceof Leaf1) return "()";
        Branch1 branch = (Branch1) tree;
        return "(" + treeToParens(branch.left) + treeToParens(branch.right) + ")";
    }

	public static boolean isBalanced(String str)
	{
		if(str.length()%2==1)
			return false;
		
		Stack<Character> stack=new Stack<Character>();
		for(int i=0;i<str.length();i++)
		{
			char ch=str.charAt(i);
			if(ch=='(')
				stack.push(ch);
			else
			{
				if(stack.isEmpty())
					return false;
				
				char pch=stack.pop();
				
				if(ch==')' && pch!='(')
					return false;
			
			}
		}
		return stack.isEmpty();
	}
	
	public static Tree1 parensToTree(String parens) 
	{
        Stack<Tree1> stack = new Stack<Tree1>();
        Stack<Character> chStack = new Stack<Character>();
        
        for(int i = 0; i < parens.length(); i++) 
        {
            if(parens.charAt(i) == '(') {
                chStack.push('(');
            }else {
                chStack.pop();
                if(i != parens.length() - 1) {
                    if(stack.size() > chStack.size()) {
                        Tree1 right = stack.pop();
                        Tree1 left = stack.pop();
                        stack.push(new Branch1(left, right));
                    }else {
                        stack.push(new Leaf1());
                    }
                }else {
                    Tree1 right = stack.pop();
                    Tree1 left = stack.pop();
                    stack.push(new Branch1(left, right));
                }
            }
        }
        return stack.peek();
    }
	
	public static void main(String[] args) {
		boolean rs=isBalanced("()(())");
		
		Tree1 tr = new Branch1(new Leaf1(), new Branch1(new Leaf1(), new Leaf1()));
        String balancedParen = treeToParens(tr);
        System.out.println(balancedParen);  
        
        Tree1 tree = parensToTree(balancedParen);
        System.out.println(tree.toString());
	}
}
