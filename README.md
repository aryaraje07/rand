TictacToe Non AI

import java.util.*;

public class TicTacToe {
    static int turn = 0;
    static int[] board = new int[9]; // 2 = blank, 3 = X, 5 = O
    static Scanner sc = new Scanner(System.in);

    static void initBoard()
     {
        Arrays.fill(board, 2);
    }

    static void display() {
        System.out.println("\nBoard:");
        for (int i = 0; i < 9; i++) {
            if (board[i] == 3)
                System.out.print("X ");
            else if (board[i] == 5)
                System.out.print("O ");
            else
                System.out.print("B ");

            if ((i + 1) % 3 == 0)
                System.out.println();
        }
        System.out.println();
    }

    static void go(int x) {
        if (x < 0 || x > 8 || board[x] != 2) return;
        if (turn % 2 == 0)
            board[x] = 3; 
        else
            board[x] = 5; 
        turn++;
    }

    static int make2() {
        if (board[4] == 2)
            return 4;
        for (int i = 1; i <= 7; i += 2) {
            if (board[i] == 2)
                return i;
        }
        return -1;
    }

    static int poswin(int n) {
        int[][] w = {
            {0, 1, 2}, {0, 3, 6}, {0, 4, 8}, {1, 4, 7},
            {2, 5, 8}, {2, 4, 6}, {3, 4, 5}, {6, 7, 8}
        };
        for (int[] line : w) {
            int prod = board[line[0]] * board[line[1]] * board[line[2]];
            int target = (n == 3) ? 18 : 50;
            if (prod == target) {
                for (int pos : line) {
                    if (board[pos] == 2)
                        return pos;
                }
            }
        }
        return -1;
    }

    static boolean iswin(int n) {
        int[][] w = {
            {0, 1, 2}, {0, 3, 6}, {0, 4, 8}, {1, 4, 7},
            {2, 5, 8}, {2, 4, 6}, {3, 4, 5}, {6, 7, 8}
        };
        for (int[] line : w) {
            int prod = board[line[0]] * board[line[1]] * board[line[2]];
            if ((n == 3 && prod == 27) || (n == 5 && prod == 125))
                return true;
        }
        return false;
    }

    public static void main(String[] args) {
        initBoard();
        int x, p, m;

        System.out.print("Enter position for X (0-8): ");
        x = sc.nextInt();
        go(x);
        display();

        m = make2();
        go(m);
        display();

        System.out.print("Enter position for X (0-8): ");
        x = sc.nextInt();
        go(x);
        display();

        p = poswin(3);
        if (p != -1)
            go(p);
        else
            go(make2());
        display();

        System.out.print("Enter position for X (0-8): ");
        x = sc.nextInt();
        go(x);
        display();

        if (iswin(3)) {
            System.out.println("You WIN!!!");
            return;
        }

        p = poswin(5);
        if (p != -1)
            go(p);
        else if (poswin(3) != -1)
            go(poswin(3));
        else
            go(make2());
        display();

        if (iswin(5)) {
            System.out.println("I WIN!!!");
            return;
        }

        System.out.print("Enter position for X (0-8): ");
        x = sc.nextInt();
        go(x);
        display();

        if (iswin(3)) {
            System.out.println("You WIN!!!");
            return;
        }

        p = poswin(5);
        if (p != -1)
            go(p);
        else if (poswin(3) != -1)
            go(poswin(3));
        else
            go(make2());
        display();

        if (iswin(5)) {
            System.out.println("I WIN!!!");
            return;
        }

        System.out.print("Enter position for X (0-8): ");
        x = sc.nextInt();
        go(x);
        display();

        if (iswin(3)) {
            System.out.println("You WIN!!!");
            return;
        }

        System.out.println("DRAW!!!");
    }
}


Tic TAc TOe AI

import java.util.*;

public class TicTacToeAI {
    static final char HUMAN = 'X';
    static final char AI = 'O';
    static final char EMPTY = ' ';

    static void displayBoard(char[] board) {
        System.out.println();
        for (int i = 0; i < 9; i++) {
            System.out.print((board[i] == EMPTY ? (char) ('0' + i) : board[i]) + " ");
            if ((i + 1) % 3 == 0) System.out.println();
        }
        System.out.println();
    }

    static char checkWinner(char[] board) {
        int[][] winLines = {
            {0, 1, 2}, {3, 4, 5}, {6, 7, 8}, // rows
            {0, 3, 6}, {1, 4, 7}, {2, 5, 8}, // cols
            {0, 4, 8}, {2, 4, 6}              // diagonals
        };

        for (int[] line : winLines) {
            if (board[line[0]] != EMPTY &&
                board[line[0]] == board[line[1]] &&
                board[line[1]] == board[line[2]]) {
                return board[line[0]];
            }
        }

        for (char c : board) {
            if (c == EMPTY) return EMPTY;
        }
        return 'D'; 
    }

    static int minimax(char[] board, boolean isAI) {
        char winner = checkWinner(board);
        if (winner == AI) return 10;
        if (winner == HUMAN) return -10;
        if (winner == 'D') return 0;

        if (isAI) {
            int best = Integer.MIN_VALUE;
            for (int i = 0; i < 9; i++) {
                if (board[i] == EMPTY) {
                    board[i] = AI;
                    best = Math.max(best, minimax(board, false));
                    board[i] = EMPTY;
                }
            }
            return best;
        } else {
            int best = Integer.MAX_VALUE;
            for (int i = 0; i < 9; i++) {
                if (board[i] == EMPTY) {
                    board[i] = HUMAN;
                    best = Math.min(best, minimax(board, true));
                    board[i] = EMPTY;
                }
            }
            return best;
        }
    }

    static int findBestMove(char[] board) {
        int bestVal = Integer.MIN_VALUE;
        int bestMove = -1;

        for (int i = 0; i < 9; i++) {
            if (board[i] == EMPTY) {
                board[i] = AI;
                int moveVal = minimax(board, false);
                board[i] = EMPTY;

                if (moveVal > bestVal) {
                    bestMove = i;
                    bestVal = moveVal;
                }
            }
        }
        return bestMove;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        char[] board = new char[9];
        Arrays.fill(board, EMPTY);

        System.out.println("Tic Tac Toe using Minimax AI");
        System.out.println("You are X, AI is O");
        displayBoard(board);

        while (true) {
            // Human move
            System.out.print("Enter your move (0-8): ");
            int move = sc.nextInt();

            if (move < 0 || move >= 9 || board[move] != EMPTY) {
                System.out.println("Invalid move, try again.");
                continue;
            }

            board[move] = HUMAN;
            displayBoard(board);

            if (checkWinner(board) == HUMAN) {
                System.out.println("You WIN!!!");
                break;
            }
            if (checkWinner(board) == 'D') {
                System.out.println("It's a DRAW!");
                break;
            }

            // AI move
            int aiMove = findBestMove(board);
            board[aiMove] = AI;
            System.out.println("AI chooses position " + aiMove);
            displayBoard(board);

            if (checkWinner(board) == AI) {
                System.out.println("AI WINS!!!");
                break;
            }
            if (checkWinner(board) == 'D') {
                System.out.println("It's a DRAW!");
                break;
            }
        }
        sc.close();
    }
}







   PS01_TicTacToe.java (stand-alone, robust, input-checked)
import java.util.*;

/** PS01: Non-AI two-player TicTacToe (no shared code, robust I/O, validation). */
public class PS01_TicTacToe {
    static final class Board {
        private final char[] b = "_________".toCharArray(); // '_' means empty
        private char turn = 'X';

        boolean isGameOver() { return winner() != '_' || !hasEmpty(); }
        boolean hasEmpty() { for (char c : b) if (c == '_') return true; return false; }
        char winner() {
            int[][] L = { {0,1,2},{3,4,5},{6,7,8},{0,3,6},{1,4,7},{2,5,8},{0,4,8},{2,4,6} };
            for (int[] l : L) if (b[l[0]] != '_' && b[l[0]] == b[l[1]] && b[l[1]] == b[l[2]]) return b[l[0]];
            return '_';
        }
        boolean move(int pos) {
            if (pos < 0 || pos > 8 || b[pos] != '_') return false;
            b[pos] = turn; turn = (turn == 'X') ? 'O' : 'X'; return true;
        }
        char turn() { return turn; }
        void print() {
            for (int i = 0; i < 9; i++) {
                System.out.print(b[i] == '_' ? '.' : b[i]);
                if (i % 3 == 2) System.out.println();
            }
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Board g = new Board();
        System.out.println("TicTacToe (cells 0..8). Example row-major indices:");
        System.out.println("0 1 2\n3 4 5\n6 7 8\n");

        while (!g.isGameOver()) {
            g.print();
            System.out.print("Player " + g.turn() + " move [0-8]: ");
            if (!sc.hasNextInt()) { System.out.println("Please enter an integer 0..8."); sc.next(); continue; }
            int p = sc.nextInt();
            if (!g.move(p)) System.out.println("Invalid move. Try again.");
        }
        g.print();
        char w = g.winner();
        System.out.println(w == '_' ? "Result: Draw" : "Winner: " + w);
    }
}

PS02_WaterJug_BFS.java (clean BFS with full helpers)
import java.util.*;

/** PS02: Water-Jug using BFS (stand-alone). */
public class PS02_WaterJug_BFS {
    /** Immutable state of the two jugs. */
    static final class State {
        final int a, b;          // current volumes
        final int A, B, T;       // capacities A,B and target T
        State(int a, int b, int A, int B, int T) { this.a=a; this.b=b; this.A=A; this.B=B; this.T=T; }
        boolean isGoal() { return a == T || b == T; }
        boolean isValid() { return 0 <= a && a <= A && 0 <= b && b <= B; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return a==s.a && b==s.b; }
        @Override public int hashCode(){ return Objects.hash(a,b); }
        @Override public String toString(){ return "(" + a + "," + b + ")"; }
    }

    /** Generate all valid successors with action labels. */
    static List<Map.Entry<String, State>> successors(State s) {
        List<Map.Entry<String, State>> r = new ArrayList<>(6);
        // fill A
        r.add(Map.entry("fill A", new State(s.A, s.b, s.A, s.B, s.T)));
        // fill B
        r.add(Map.entry("fill B", new State(s.a, s.B, s.A, s.B, s.T)));
        // empty A
        r.add(Map.entry("empty A", new State(0, s.b, s.A, s.B, s.T)));
        // empty B
        r.add(Map.entry("empty B", new State(s.a, 0, s.A, s.B, s.T)));
        // pour A -> B
        int m = Math.min(s.a, s.B - s.b);
        r.add(Map.entry("pour A->B", new State(s.a - m, s.b + m, s.A, s.B, s.T)));
        // pour B -> A
        int m2 = Math.min(s.b, s.A - s.a);
        r.add(Map.entry("pour B->A", new State(s.a + m2, s.b - m2, s.A, s.B, s.T)));

        // keep only valid states (capacity bounds)
        r.removeIf(e -> !e.getValue().isValid());
        return r;
    }

    /** Reconstruct and print path. */
    static void printSolution(String title, State start, State goal, Map<State, State> parent, Map<State, String> action) {
        LinkedList<String> path = new LinkedList<>();
        for (State x = goal; x != null; x = parent.get(x)) {
            String line = (x.equals(start)) ? x.toString() : action.get(x) + " -> " + x;
            path.addFirst(line);
        }
        System.out.println("=== " + title + " ===");
        path.forEach(System.out::println);
        System.out.println("Steps: " + (path.size() - 1));
    }

    /** BFS over the state space. */
    static Optional<State> bfs(State start) {
        Queue<State> q = new ArrayDeque<>();
        Set<State> seen = new HashSet<>();
        q.add(start); seen.add(start);
        while (!q.isEmpty()) {
            State s = q.remove();
            if (s.isGoal()) return Optional.of(s);
            for (var e : successors(s)) {
                State t = e.getValue();
                if (seen.add(t)) { q.add(t); parent.put(t, s); action.put(t, e.getKey()); }
            }
        }
        return Optional.empty();
    }

    // storage for path reconstruction
    static Map<State, State> parent = new HashMap<>();
    static Map<State, String> action = new HashMap<>();

    public static void main(String[] args) {
        // CLI: A B T  (defaults: 4 3 2)
        int A = args.length > 0 ? Integer.parseInt(args[0]) : 4;
        int B = args.length > 1 ? Integer.parseInt(args[1]) : 3;
        int T = args.length > 2 ? Integer.parseInt(args[2]) : 2;

        State start = new State(0,0,A,B,T);
        var fin = bfs(start);
        if (fin.isEmpty()) {
            System.out.println("No solution.");
        } else {
            printSolution("Water-Jug BFS", start, fin.get(), parent, action);
        }
    }
}

PS07_WaterJug_DFS.java (depth-limited DFS with full guards)
import java.util.*;

/** PS07: Water-Jug with depth-limited DFS (stand-alone). */
public class PS07_WaterJug_DFS {
    static final class State {
        final int a,b,A,B,T;
        State(int a,int b,int A,int B,int T){ this.a=a; this.b=b; this.A=A; this.B=B; this.T=T; }
        boolean isGoal(){ return a==T || b==T; }
        boolean isValid(){ return 0<=a && a<=A && 0<=b && b<=B; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return a==s.a && b==s.b; }
        @Override public int hashCode(){ return Objects.hash(a,b); }
        @Override public String toString(){ return "(" + a + "," + b + ")"; }
    }

    static List<Map.Entry<String, State>> successors(State s){
        List<Map.Entry<String, State>> r=new ArrayList<>(6);
        r.add(Map.entry("fill A", new State(s.A,s.b,s.A,s.B,s.T)));
        r.add(Map.entry("fill B", new State(s.a,s.B,s.A,s.B,s.T)));
        r.add(Map.entry("empty A", new State(0,s.b,s.A,s.B,s.T)));
        r.add(Map.entry("empty B", new State(s.a,0,s.A,s.B,s.T)));
        int m=Math.min(s.a, s.B-s.b); r.add(Map.entry("pour A->B", new State(s.a-m,s.b+m,s.A,s.B,s.T)));
        int m2=Math.min(s.b, s.A-s.a); r.add(Map.entry("pour B->A", new State(s.a+m2,s.b-m2,s.A,s.B,s.T)));
        r.removeIf(e->!e.getValue().isValid());
        return r;
    }

    static Map<State,State> parent = new HashMap<>();
    static Map<State,String> action = new HashMap<>();
    static Set<State> visiting = new HashSet<>();

    static boolean dls(State s, int limit){
        if(s.isGoal()) return true;
        if(limit==0) return false;
        visiting.add(s);
        for(var e: successors(s)){
            State t=e.getValue();
            if(visiting.contains(t)) continue; // avoid local cycles
            parent.put(t,s); action.put(t,e.getKey());
            if(dls(t, limit-1)) return true;
        }
        visiting.remove(s);
        return false;
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal; x!=null; x=parent.get(x)){
            String line = x.equals(start) ? x.toString() : action.get(x)+" -> "+x;
            path.addFirst(line);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Depth: " + (path.size()-1));
    }

    public static void main(String[] args){
        int A = args.length>0?Integer.parseInt(args[0]):4;
        int B = args.length>1?Integer.parseInt(args[1]):3;
        int T = args.length>2?Integer.parseInt(args[2]):2;
        int L = args.length>3?Integer.parseInt(args[3]):50;

        State start=new State(0,0,A,B,T);
        if(dls(start, L)){
            // find any goal we reached
            State g=null;
            for(State s: action.keySet()) if(s.isGoal()){ g=s; break; }
            if(g!=null) printSolution("Water-Jug DFS (limit="+L+")", start, g);
            else System.out.println("Goal reached but not recorded (unexpected).");
        }else{
            System.out.println("No solution within depth "+L+".");
        }
    }
}

PS12_WaterJug_DLS.java (your sample’s spirit, tightened + CLI)
import java.util.*;

/** PS12: Water-Jug with strict Depth-Limited Search (stand-alone). */
public class PS12_WaterJug_DLS {
    static final class State {
        final int a,b,A,B,T;
        State(int a,int b,int A,int B,int T){ this.a=a; this.b=b; this.A=A; this.B=B; this.T=T; }
        boolean isGoal(){ return a==T || b==T; }
        boolean isValid(){ return 0<=a && a<=A && 0<=b && b<=B; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return a==s.a && b==s.b; }
        @Override public int hashCode(){ return Objects.hash(a,b); }
        @Override public String toString(){ return "(" + a + "," + b + ")"; }
    }

    static List<Map.Entry<String, State>> successors(State s){
        List<Map.Entry<String, State>> r=new ArrayList<>(6);
        r.add(Map.entry("fill A", new State(s.A,s.b,s.A,s.B,s.T)));
        r.add(Map.entry("fill B", new State(s.a,s.B,s.A,s.B,s.T)));
        r.add(Map.entry("empty A", new State(0,s.b,s.A,s.B,s.T)));
        r.add(Map.entry("empty B", new State(s.a,0,s.A,s.B,s.T)));
        int m=Math.min(s.a, s.B-s.b); r.add(Map.entry("pour A->B", new State(s.a-m,s.b+m,s.A,s.B,s.T)));
        int m2=Math.min(s.b, s.A-s.a); r.add(Map.entry("pour B->A", new State(s.a+m2,s.b-m2,s.A,s.B,s.T)));
        r.removeIf(e->!e.getValue().isValid());
        return r;
    }

    static Map<State,State> parent = new HashMap<>();
    static Map<State,String> action = new HashMap<>();

    static boolean dls(State s, int limit){
        if(s.isGoal()) return true;
        if(limit==0) return false;
        for(var e: successors(s)){
            State t=e.getValue();
            if(parent.containsKey(t)) continue;
            parent.put(t,s); action.put(t,e.getKey());
            if(dls(t, limit-1)) return true;
        }
        return false;
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal; x!=null; x=parent.get(x)){
            String line = x.equals(start) ? x.toString() : action.get(x)+" -> "+x;
            path.addFirst(line);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Depth: " + (path.size()-1));
    }

    public static void main(String[] args){
        int A = args.length>0?Integer.parseInt(args[0]):4;
        int B = args.length>1?Integer.parseInt(args[1]):3;
        int T = args.length>2?Integer.parseInt(args[2]):2;
        int L = args.length>3?Integer.parseInt(args[3]):3;

        State start=new State(0,0,A,B,T);
        if(dls(start, L)){
            State g=null; for(State s: action.keySet()) if(s.isGoal()){ g=s; break; }
            printSolution("Water-Jug DLS (depth="+L+")", start, g);
        }else{
            System.out.println("No solution within depth "+L+".");
        }
    }
}

PS17_WaterJug_BestFirst.java (Greedy Best-First with explicit heuristic)
import java.util.*;

/** PS17: Water-Jug Greedy Best-First Search (stand-alone). */
public class PS17_WaterJug_BestFirst {
    static final class State {
        final int a,b,A,B,T;
        State(int a,int b,int A,int B,int T){ this.a=a; this.b=b; this.A=A; this.B=B; this.T=T; }
        boolean isGoal(){ return a==T || b==T; }
        boolean isValid(){ return 0<=a && a<=A && 0<=b && b<=B; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return a==s.a && b==s.b; }
        @Override public int hashCode(){ return Objects.hash(a,b); }
        @Override public String toString(){ return "(" + a + "," + b + ")"; }
    }

    static int h(State s){ return Math.min(Math.abs(s.a - s.T), Math.abs(s.b - s.T)); }

    static List<Map.Entry<String, State>> successors(State s){
        List<Map.Entry<String, State>> r=new ArrayList<>(6);
        r.add(Map.entry("fill A", new State(s.A,s.b,s.A,s.B,s.T)));
        r.add(Map.entry("fill B", new State(s.a,s.B,s.A,s.B,s.T)));
        r.add(Map.entry("empty A", new State(0,s.b,s.A,s.B,s.T)));
        r.add(Map.entry("empty B", new State(s.a,0,s.A,s.B,s.T)));
        int m=Math.min(s.a, s.B-s.b); r.add(Map.entry("pour A->B", new State(s.a-m,s.b+m,s.A,s.B,s.T)));
        int m2=Math.min(s.b, s.A-s.a); r.add(Map.entry("pour B->A", new State(s.a+m2,s.b-m2,s.A,s.B,s.T)));
        r.removeIf(e->!e.getValue().isValid());
        return r;
    }

    static Map<State,State> parent = new HashMap<>();
    static Map<State,String> action = new HashMap<>();

    public static void main(String[] args){
        int A = args.length>0?Integer.parseInt(args[0]):4;
        int B = args.length>1?Integer.parseInt(args[1]):3;
        int T = args.length>2?Integer.parseInt(args[2]):2;

        State start=new State(0,0,A,B,T);
        PriorityQueue<State> open = new PriorityQueue<>(Comparator.comparingInt(PS17_WaterJug_BestFirst::h));
        Set<State> closed = new HashSet<>();
        open.add(start); parent.put(start,null);

        while(!open.isEmpty()){
            State s=open.poll();
            if(s.isGoal()){ printSolution("Water-Jug Greedy Best-First", start, s); return; }
            if(!closed.add(s)) continue;
            for(var e: successors(s)){
                State t=e.getValue();
                if(parent.containsKey(t)) continue;
                parent.put(t,s); action.put(t,e.getKey());
                open.add(t);
            }
        }
        System.out.println("No solution.");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal; x!=null; x=parent.get(x)){
            String line = x.equals(start) ? x.toString() : action.get(x)+" -> "+x;
            path.addFirst(line);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Steps: " + (path.size()-1));
    }
}

PS23_WaterJug_AStar.java (A* with admissible heuristic)
import java.util.*;

/** PS23: Water-Jug A* Search (stand-alone). */
public class PS23_WaterJug_AStar {
    static final class State {
        final int a,b,A,B,T;
        State(int a,int b,int A,int B,int T){ this.a=a; this.b=b; this.A=A; this.B=B; this.T=T; }
        boolean isGoal(){ return a==T || b==T; }
        boolean isValid(){ return 0<=a && a<=A && 0<=b && b<=B; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return a==s.a && b==s.b; }
        @Override public int hashCode(){ return Objects.hash(a,b); }
        @Override public String toString(){ return "(" + a + "," + b + ")"; }
    }
    static int h(State s){ return Math.min(Math.abs(s.a - s.T), Math.abs(s.b - s.T)); }

    static List<Map.Entry<String, State>> successors(State s){
        List<Map.Entry<String, State>> r=new ArrayList<>(6);
        r.add(Map.entry("fill A", new State(s.A,s.b,s.A,s.B,s.T)));
        r.add(Map.entry("fill B", new State(s.a,s.B,s.A,s.B,s.T)));
        r.add(Map.entry("empty A", new State(0,s.b,s.A,s.B,s.T)));
        r.add(Map.entry("empty B", new State(s.a,0,s.A,s.B,s.T)));
        int m=Math.min(s.a, s.B-s.b); r.add(Map.entry("pour A->B", new State(s.a-m,s.b+m,s.A,s.B,s.T)));
        int m2=Math.min(s.b, s.A-s.a); r.add(Map.entry("pour B->A", new State(s.a+m2,s.b-m2,s.A,s.B,s.T)));
        r.removeIf(e->!e.getValue().isValid());
        return r;
    }

    static Map<State,State> parent = new HashMap<>();
    static Map<State,String> action = new HashMap<>();
    static Map<State,Integer> gScore = new HashMap<>();

    public static void main(String[] args){
        int A = args.length>0?Integer.parseInt(args[0]):4;
        int B = args.length>1?Integer.parseInt(args[1]):3;
        int T = args.length>2?Integer.parseInt(args[2]):2;

        State start=new State(0,0,A,B,T);
        PriorityQueue<State> open = new PriorityQueue<>(Comparator.comparingInt(s -> gScore.getOrDefault(s, 0) + h(s)));
        gScore.put(start, 0); parent.put(start, null); open.add(start);

        while(!open.isEmpty()){
            State s=open.poll();
            if(s.isGoal()){ printSolution("Water-Jug A*", start, s); return; }
            for(var e: successors(s)){
                State t=e.getValue();
                int tentativeG = gScore.get(s) + 1; // unit cost per action
                if (tentativeG < gScore.getOrDefault(t, Integer.MAX_VALUE)) {
                    gScore.put(t, tentativeG); parent.put(t, s); action.put(t, e.getKey());
                    // update priority
                    open.remove(t); open.add(t);
                }
            }
        }
        System.out.println("No solution.");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal; x!=null; x=parent.get(x)){
            String line = x.equals(start) ? x.toString() : action.get(x)+" -> "+x;
            path.addFirst(line);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Cost: " + (path.size()-1));
    }
}

PS03_EightPuzzle_BFS.java (expanded, with input + guards)
import java.util.*;

/** PS03: 8-Puzzle BFS (stand-alone). Pass 9 integers for start or uses default. */
public class PS03_EightPuzzle_BFS {
    static final class State {
        final int[] t; State(int[] t){ this.t=t; }
        boolean isGoal(){ return Arrays.equals(t, new int[]{1,2,3,4,5,6,7,8,0}); }
        int zero(){ for(int i=0;i<9;i++) if(t[i]==0) return i; return -1; }
        State swap(int i,int j){ int[] c=t.clone(); int v=c[i]; c[i]=c[j]; c[j]=v; return new State(c); }
        @Override public boolean equals(Object o){ return (o instanceof State s) && Arrays.equals(t, s.t); }
        @Override public int hashCode(){ return Arrays.hashCode(t); }
        @Override public String toString(){ return Arrays.toString(t); }
    }
    static final int[][] MOVES={{1,3},{0,2,4},{1,5},{0,4,6},{1,3,5,7},{2,4,8},{3,7},{4,6,8},{5,7}};

    static List<Map.Entry<String, State>> successors(State s){
        int z = s.zero();
        List<Map.Entry<String, State>> r = new ArrayList<>();
        for(int m : MOVES[z]) r.add(Map.entry("swap "+z+"-"+m, s.swap(z,m)));
        return r;
    }

    static Map<State,State> parent = new HashMap<>();
    static Map<State,String> action = new HashMap<>();

    public static void main(String[] args){
        int[] start = args.length==9 ? Arrays.stream(args).mapToInt(Integer::parseInt).toArray()
                                     : new int[]{1,2,3,4,0,6,7,5,8};
        State st = new State(start);

        Queue<State> q=new ArrayDeque<>(); Set<State> seen=new HashSet<>();
        q.add(st); seen.add(st); parent.put(st,null);

        while(!q.isEmpty()){
            State s=q.remove();
            if(s.isGoal()){ printSolution("8-Puzzle BFS", st, s); return; }
            for(var e: successors(s)){
                State t=e.getValue();
                if(seen.add(t)){ parent.put(t,s); action.put(t,e.getKey()); q.add(t); }
            }
        }
        System.out.println("No solution (state may be unsolvable).");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal; x!=null; x=parent.get(x)){
            String line = x.equals(start) ? x.toString() : action.get(x)+" -> "+x;
            path.addFirst(line);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Steps: " + (path.size()-1));
    }
}

PS24_EightPuzzle_AStar.java (A* with Manhattan heuristic)
import java.util.*;

/** PS24: 8-Puzzle A* (stand-alone). */
public class PS24_EightPuzzle_AStar {
    static final class State {
        final int[] t; State(int[] t){ this.t=t; }
        boolean isGoal(){ return Arrays.equals(t, new int[]{1,2,3,4,5,6,7,8,0}); }
        int zero(){ for(int i=0;i<9;i++) if(t[i]==0) return i; return -1; }
        State swap(int i,int j){ int[] c=t.clone(); int v=c[i]; c[i]=c[j]; c[j]=v; return new State(c); }
        @Override public boolean equals(Object o){ return (o instanceof State s) && Arrays.equals(t, s.t); }
        @Override public int hashCode(){ return Arrays.hashCode(t); }
        @Override public String toString(){ return Arrays.toString(t); }
    }
    static final int[][] MOVES={{1,3},{0,2,4},{1,5},{0,4,6},{1,3,5,7},{2,4,8},{3,7},{4,6,8},{5,7}};

    static int h(State s){
        int H=0; for(int i=0;i<9;i++){ int v=s.t[i]; if(v==0) continue; int gi=v-1;
            H += Math.abs(i/3 - gi/3) + Math.abs(i%3 - gi%3);
        } return H;
    }

    static List<Map.Entry<String, State>> successors(State s){
        int z = s.zero();
        List<Map.Entry<String, State>> r = new ArrayList<>();
        for(int m : MOVES[z]) r.add(Map.entry("swap "+z+"-"+m, s.swap(z,m)));
        return r;
    }

    static Map<State,State> parent = new HashMap<>();
    static Map<State,String> action = new HashMap<>();
    static Map<State,Integer> gScore = new HashMap<>();

    public static void main(String[] args){
        int[] start = args.length==9 ? Arrays.stream(args).mapToInt(Integer::parseInt).toArray()
                                     : new int[]{1,2,3,4,0,6,7,5,8};
        State st = new State(start);
        PriorityQueue<State> open=new PriorityQueue<>(Comparator.comparingInt(s->gScore.getOrDefault(s,0)+h(s)));
        gScore.put(st,0); parent.put(st,null); open.add(st);

        while(!open.isEmpty()){
            State s=open.poll();
            if(s.isGoal()){ printSolution("8-Puzzle A*", st, s); return; }
            for(var e: successors(s)){
                State t=e.getValue();
                int ng=gScore.get(s)+1;
                if(ng < gScore.getOrDefault(t, Integer.MAX_VALUE)){
                    gScore.put(t, ng); parent.put(t, s); action.put(t, e.getKey());
                    open.remove(t); open.add(t);
                }
            }
        }
        System.out.println("No solution (state may be unsolvable).");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal; x!=null; x=parent.get(x)){
            String line = x.equals(start) ? x.toString() : action.get(x)+" -> "+x;
            path.addFirst(line);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Cost: " + (path.size()-1));
    }
}

PS04_Maze_BFS.java (grid BFS with walls, Manhattan trace)
import java.util.*;

/** PS04: Maze BFS on a grid (stand-alone). */
public class PS04_Maze_BFS {
    static final class State {
        final int r,c,gr,gc; final boolean[][] w;
        State(int r,int c,boolean[][] w,int gr,int gc){ this.r=r; this.c=c; this.w=w; this.gr=gr; this.gc=gc; }
        boolean isGoal(){ return r==gr && c==gc; }
        boolean inside(int nr,int nc){ return nr>=0 && nc>=0 && nr<w.length && nc<w[0].length; }
        boolean free(int nr,int nc){ return !w[nr][nc]; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return r==s.r&&c==s.c; }
        @Override public int hashCode(){ return Objects.hash(r,c); }
        @Override public String toString(){ return "("+r+","+c+")"; }
    }

    static final int[] DR={-1,1,0,0};
    static final int[] DC={0,0,-1,1};
    static final String[] AN={"up","down","left","right"};

    static List<Map.Entry<String, State>> successors(State s){
        List<Map.Entry<String, State>> r=new ArrayList<>(4);
        for(int i=0;i<4;i++){
            int nr=s.r+DR[i], nc=s.c+DC[i];
            if(s.inside(nr,nc) && s.free(nr,nc)){
                r.add(Map.entry(AN[i], new State(nr,nc,s.w,s.gr,s.gc)));
            }
        }
        return r;
    }

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();

    public static void main(String[] args){
        // Build a small maze with a couple of walls
        int R = 5, C = 7;
        boolean[][] walls = new boolean[R][C];
        walls[1][1] = walls[1][2] = walls[1][3] = true;
        walls[3][3] = walls[3][4] = true;

        State start = new State(0,0,walls,4,6);
        Queue<State> q=new ArrayDeque<>(); Set<State> seen=new HashSet<>();
        q.add(start); seen.add(start); parent.put(start,null);

        while(!q.isEmpty()){
            State s=q.remove();
            if(s.isGoal()){ printSolution("Maze BFS", start, s); return; }
            for(var e: successors(s)){
                State t=e.getValue();
                if(seen.add(t)){ parent.put(t,s); action.put(t,e.getKey()); q.add(t); }
            }
        }
        System.out.println("No path.");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal; x!=null; x=parent.get(x)){
            String line = x.equals(start) ? x.toString() : action.get(x)+" -> "+x;
            path.addFirst(line);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Steps: " + (path.size()-1));
    }
}

PS25_Maze_AStar.java (grid A* with Manhattan heuristic)
import java.util.*;

/** PS25: Maze A* (stand-alone). */
public class PS25_Maze_AStar {
    static final class State {
        final int r,c,gr,gc; final boolean[][] w;
        State(int r,int c,boolean[][] w,int gr,int gc){ this.r=r; this.c=c; this.w=w; this.gr=gr; this.gc=gc; }
        boolean isGoal(){ return r==gr && c==gc; }
        boolean inside(int nr,int nc){ return nr>=0 && nc>=0 && nr<w.length && nc<w[0].length; }
        boolean free(int nr,int nc){ return !w[nr][nc]; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return r==s.r&&c==s.c; }
        @Override public int hashCode(){ return Objects.hash(r,c); }
        @Override public String toString(){ return "("+r+","+c+")"; }
    }

    static final int[] DR={-1,1,0,0};
    static final int[] DC={0,0,-1,1};
    static final String[] AN={"up","down","left","right"};

    static int h(State s){ return Math.abs(s.r - s.gr) + Math.abs(s.c - s.gc); }

    static List<Map.Entry<String, State>> successors(State s){
        List<Map.Entry<String, State>> r=new ArrayList<>(4);
        for(int i=0;i<4;i++){
            int nr=s.r+DR[i], nc=s.c+DC[i];
            if(s.inside(nr,nc) && s.free(nr,nc)){
                r.add(Map.entry(AN[i], new State(nr,nc,s.w,s.gr,s.gc)));
            }
        }
        return r;
    }

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();
    static Map<State,Integer> g=new HashMap<>();

    public static void main(String[] args){
        int R=5, C=7;
        boolean[][] walls = new boolean[R][C];
        walls[1][1] = walls[1][2] = walls[1][3] = true;
        walls[3][3] = walls[3][4] = true;

        State start = new State(0,0,walls,4,6);
        PriorityQueue<State> open = new PriorityQueue<>(Comparator.comparingInt(s -> g.getOrDefault(s,0) + h(s)));
        g.put(start, 0); parent.put(start, null); open.add(start);

        while(!open.isEmpty()){
            State s=open.poll();
            if(s.isGoal()){ printSolution("Maze A*", start, s); return; }
            for(var e: successors(s)){
                State t=e.getValue();
                int ng = g.get(s) + 1;
                if(ng < g.getOrDefault(t, Integer.MAX_VALUE)){
                    g.put(t, ng); parent.put(t, s); action.put(t, e.getKey());
                    open.remove(t); open.add(t);
                }
            }
        }
        System.out.println("No path.");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal; x!=null; x=parent.get(x)){
            String line = x.equals(start) ? x.toString() : action.get(x)+" -> "+x;
            path.addFirst(line);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Cost: " + (path.size()-1));
    }
}

PS05_MissionariesCannibals_BFS.java (proper constraints + trace)
import java.util.*;

/** PS05: Missionaries & Cannibals BFS (stand-alone). */
public class PS05_MissionariesCannibals_BFS {
    static final class State {
        final int ml, cl; // left bank
        final int mr, cr; // right bank (derived)
        final int n;      // total per side initially
        final int boat;   // 0=left, 1=right
        State(int ml,int cl,int boat,int n){ this.ml=ml; this.cl=cl; this.boat=boat; this.n=n; this.mr=n-ml; this.cr=n-cl; }
        boolean valid(){
            boolean leftOK = (ml==0 || ml>=cl);
            boolean rightOK = (mr==0 || mr>=cr);
            return ml>=0 && cl>=0 && mr>=0 && cr>=0 && leftOK && rightOK;
        }
        boolean isGoal(){ return ml==0 && cl==0 && boat==1; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return ml==s.ml && cl==s.cl && boat==s.boat; }
        @Override public int hashCode(){ return Objects.hash(ml,cl,boat); }
        @Override public String toString(){ return "L("+ml+","+cl+")  R("+mr+","+cr+")  Boat:"+(boat==0?"L":"R"); }
    }

    static final int[][] MOVES={{1,0},{2,0},{0,1},{0,2},{1,1}}; // (M,C)

    static List<Map.Entry<String, State>> successors(State s){
        List<Map.Entry<String, State>> r=new ArrayList<>();
        int dir = s.boat==0 ? -1 : +1; // left->right uses -1 on left side
        String side = (s.boat==0 ? "L->R" : "R->L");
        for(int[] m: MOVES){
            State t=new State(s.ml + dir*m[0], s.cl + dir*m[1], 1 - s.boat, s.n);
            if(t.valid()){
                r.add(Map.entry(side + " " + Arrays.toString(m), t));
            }
        }
        return r;
    }

    static Map<State,State> parent = new HashMap<>();
    static Map<State,String> action = new HashMap<>();

    public static void main(String[] args){
        int n = args.length>0 ? Integer.parseInt(args[0]) : 3;
        State start = new State(n,n,0,n);

        Queue<State> q=new ArrayDeque<>(); Set<State> seen=new HashSet<>();
        q.add(start); seen.add(start); parent.put(start,null);

        while(!q.isEmpty()){
            State s=q.remove();
            if(s.isGoal()){ printSolution("M&C BFS", start, s); return; }
            for(var e: successors(s)){
                State t=e.getValue();
                if(seen.add(t)){ parent.put(t,s); action.put(t,e.getKey()); q.add(t); }
            }
        }
        System.out.println("No solution.");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal; x!=null; x=parent.get(x)){
            String line = x.equals(start) ? x.toString() : action.get(x)+" -> "+x;
            path.addFirst(line);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Steps: " + (path.size()-1));
    }
}

PS10_MissionariesCannibals_DFS.java (depth-limited; mirrors your sample but fuller)
import java.util.*;

/** PS10: Missionaries & Cannibals depth-limited DFS (stand-alone). */
public class PS10_MissionariesCannibals_DFS {
    static final class State {
        final int ml, cl, n, boat; // boat:0 L, 1 R
        State(int ml,int cl,int boat,int n){ this.ml=ml; this.cl=cl; this.boat=boat; this.n=n; }
        int mr(){ return n-ml; } int cr(){ return n-cl; }
        boolean valid(){
            boolean leftOK = (ml==0 || ml>=cl);
            boolean rightOK = (mr()==0 || mr()>=cr());
            return ml>=0 && cl>=0 && mr()>=0 && cr()>=0 && leftOK && rightOK;
        }
        boolean isGoal(){ return ml==0 && cl==0 && boat==1; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return ml==s.ml && cl==s.cl && boat==s.boat; }
        @Override public int hashCode(){ return Objects.hash(ml,cl,boat); }
        @Override public String toString(){ return "L("+ml+","+cl+")  R("+mr()+","+cr()+")  Boat:"+(boat==0?"L":"R"); }
    }

    static final int[][] MOVES={{1,0},{2,0},{0,1},{0,2},{1,1}};
    static List<Map.Entry<String, State>> successors(State s){
        List<Map.Entry<String, State>> r=new ArrayList<>();
        int dir = s.boat==0 ? -1 : +1; String side = s.boat==0 ? "L->R" : "R->L";
        for(int[] m: MOVES){
            State t = new State(s.ml + dir*m[0], s.cl + dir*m[1], 1 - s.boat, s.n);
            if(t.valid()) r.add(Map.entry(side+" "+Arrays.toString(m), t));
        }
        return r;
    }

    static Map<State,State> parent = new HashMap<>();
    static Map<State,String> action = new HashMap<>();
    static Set<State> visiting = new HashSet<>();

    static boolean dls(State s, int limit){
        if(s.isGoal()) return true;
        if(limit==0) return false;
        visiting.add(s);
        for(var e: successors(s)){
            State t=e.getValue();
            if(visiting.contains(t)) continue;
            parent.put(t,s); action.put(t,e.getKey());
            if(dls(t, limit-1)) return true;
        }
        visiting.remove(s);
        return false;
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal; x!=null; x=parent.get(x)){
            String line = x.equals(start) ? x.toString() : action.get(x)+" -> "+x;
            path.addFirst(line);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Depth: " + (path.size()-1));
    }

    public static void main(String[] args){
        int n = args.length>0 ? Integer.parseInt(args[0]) : 3;
        int L = args.length>1 ? Integer.parseInt(args[1]) : 50;
        State start = new State(n,n,0,n);

        if(dls(start, L)){
            State g=null; for(State s: action.keySet()) if(s.isGoal()){ g=s; break; }
            printSolution("M&C DFS (limit="+L+")", start, g);
        } else {
            System.out.println("No solution within depth "+L+".");
        }
    }
}

What next

If this is the level of completeness you wanted (clean structure, proper functions, validation, heuristics, tracing, CLI), I’ll deliver PS11–PS40 in the exact same expanded style (Eight-Queens across DFS/DLS/Greedy/A*, route-finding BFS/A*, CSPs, predicate demos, expert systems).

You said:
yes
ChatGPT said:

Awesome — here are the remaining stand-alone programs (PS11–PS40) in the expanded, “proper” style (clean state models, helpers, validation, tracing, CLI args).
Save each block into its own .java file exactly as titled and run independently.

PS11_EightQueens_DFS.java
import java.util.*;

/** PS11: Eight Queens — Depth-First Search with depth guard (stand-alone). */
public class PS11_EightQueens_DFS {
    static final class State {
        final int n, next;      // next row to place
        final int[] colByRow;   // partial placement: for rows [0..next-1]
        State(int n, int next, int[] colByRow){ this.n=n; this.next=next; this.colByRow=colByRow; }
        boolean isGoal(){ return next==n; }
        boolean safe(int r,int c){
            for(int i=0;i<r;i++){
                int v=colByRow[i];
                if(v==c || Math.abs(v-c)==Math.abs(i-r)) return false;
            } return true;
        }
        State place(int r,int c){
            int[] copy=colByRow.clone(); copy[r]=c; return new State(n, r+1, copy);
        }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return Arrays.equals(Arrays.copyOf(colByRow,next), Arrays.copyOf(s.colByRow,s.next)); }
        @Override public int hashCode(){ return Arrays.hashCode(Arrays.copyOf(colByRow,next)); }
        @Override public String toString(){ return Arrays.toString(Arrays.copyOf(colByRow,next)); }
    }

    static Map<State,State> parent = new HashMap<>();
    static Map<State,String> action = new HashMap<>();
    static Set<State> visiting = new HashSet<>();

    static boolean dfs(State s, int limit){
        if(s.isGoal()) return true;
        if(limit==0) return false;
        visiting.add(s);
        int r=s.next;
        for(int c=0;c<s.n;c++){
            if(!s.safe(r,c)) continue;
            State t=s.place(r,c);
            if(visiting.contains(t)) continue;
            parent.put(t,s); action.put(t,"place r"+r+" c"+c);
            if(dfs(t, limit-1)) return true;
        }
        visiting.remove(s);
        return false;
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal; x!=null; x=parent.get(x)){
            path.addFirst(x.equals(start) ? x.toString() : action.get(x)+" -> "+x);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Depth: "+(path.size()-1));
    }

    public static void main(String[] args){
        int n = args.length>0? Integer.parseInt(args[0]) : 8;
        int limit = args.length>1? Integer.parseInt(args[1]) : 64;
        State start = new State(n, 0, new int[n]);
        if(dfs(start, limit)){
            State g=null; for(State s: parent.keySet()) if(s.isGoal()){ g=s; break; }
            printSolution("Eight Queens DFS", start, g);
        }else{
            System.out.println("No solution within depth "+limit);
        }
    }
}

PS13_EightPuzzle_DLS.java
import java.util.*;

/** PS13: 8-Puzzle — Depth-Limited Search (stand-alone). */
public class PS13_EightPuzzle_DLS {
    static final class State {
        final int[] t; State(int[] t){ this.t=t; }
        boolean isGoal(){ return Arrays.equals(t, new int[]{1,2,3,4,5,6,7,8,0}); }
        int zero(){ for(int i=0;i<9;i++) if(t[i]==0) return i; return -1; }
        State swap(int i,int j){ int[] c=t.clone(); int v=c[i]; c[i]=c[j]; c[j]=v; return new State(c); }
        @Override public boolean equals(Object o){ return (o instanceof State s) && Arrays.equals(t, s.t); }
        @Override public int hashCode(){ return Arrays.hashCode(t); }
        @Override public String toString(){ return Arrays.toString(t); }
    }
    static final int[][] MOV={{1,3},{0,2,4},{1,5},{0,4,6},{1,3,5,7},{2,4,8},{3,7},{4,6,8},{5,7}};

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();
    static Set<State> visiting=new HashSet<>();

    static boolean dls(State s, int limit){
        if(s.isGoal()) return true;
        if(limit==0) return false;
        visiting.add(s);
        int z=s.zero();
        for(int m: MOV[z]){
            State t=s.swap(z,m);
            if(visiting.contains(t)) continue;
            parent.put(t,s); action.put(t,"swap "+z+"-"+m);
            if(dls(t, limit-1)) return true;
        }
        visiting.remove(s);
        return false;
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal;x!=null;x=parent.get(x)){
            path.addFirst(x.equals(start)?x.toString():action.get(x)+" -> "+x);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Depth: "+(path.size()-1));
    }

    public static void main(String[] args){
        int[] start = args.length==9 ? Arrays.stream(args).mapToInt(Integer::parseInt).toArray()
                                     : new int[]{1,2,3,4,0,6,7,5,8};
        int L = args.length==10 ? Integer.parseInt(args[9]) : 20;
        State st=new State(start);
        if(dls(st, L)){
            State g=null; for(State s: parent.keySet()) if(s.isGoal()){g=s;break;}
            printSolution("8-Puzzle DLS", st, g);
        }else{
            System.out.println("Not found within depth "+L);
        }
    }
}

PS14_Maze_DLS.java
import java.util.*;

/** PS14: Maze — Depth-Limited Search (stand-alone). */
public class PS14_Maze_DLS {
    static final class State {
        final int r,c,gr,gc; final boolean[][] w;
        State(int r,int c,boolean[][] w,int gr,int gc){ this.r=r; this.c=c; this.w=w; this.gr=gr; this.gc=gc; }
        boolean isGoal(){ return r==gr && c==gc; }
        boolean inside(int nr,int nc){ return nr>=0 && nc>=0 && nr<w.length && nc<w[0].length; }
        boolean free(int nr,int nc){ return !w[nr][nc]; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return r==s.r&&c==s.c; }
        @Override public int hashCode(){ return Objects.hash(r,c); }
        @Override public String toString(){ return "("+r+","+c+")"; }
    }
    static final int[] DR={-1,1,0,0};
    static final int[] DC={0,0,-1,1};
    static final String[] AN={"up","down","left","right"};

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();
    static Set<State> visiting=new HashSet<>();

    static boolean dls(State s, int limit){
        if(s.isGoal()) return true;
        if(limit==0) return false;
        visiting.add(s);
        for(int i=0;i<4;i++){
            int nr=s.r+DR[i], nc=s.c+DC[i];
            if(!s.inside(nr,nc) || !s.free(nr,nc)) continue;
            State t=new State(nr,nc,s.w,s.gr,s.gc);
            if(visiting.contains(t)) continue;
            parent.put(t,s); action.put(t,AN[i]);
            if(dls(t, limit-1)) return true;
        }
        visiting.remove(s);
        return false;
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal;x!=null;x=parent.get(x)){
            path.addFirst(x.equals(start)?x.toString():action.get(x)+" -> "+x);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Depth: "+(path.size()-1));
    }

    public static void main(String[] args){
        int R=5,C=7;
        boolean[][] walls=new boolean[R][C];
        walls[1][1]=walls[1][2]=walls[1][3]=true;
        walls[3][3]=walls[3][4]=true;
        int L = args.length>0? Integer.parseInt(args[0]) : 15;

        State st=new State(0,0,walls,4,6);
        if(dls(st, L)){
            State g=null; for(State s: parent.keySet()) if(s.isGoal()){g=s;break;}
            printSolution("Maze DLS", st, g);
        }else{
            System.out.println("Not found within depth "+L);
        }
    }
}

PS15_MissionariesCannibals_DLS.java
import java.util.*;

/** PS15: Missionaries & Cannibals — Depth-Limited Search (stand-alone). */
public class PS15_MissionariesCannibals_DLS {
    static final class State {
        final int ml,cl,n,boat; // boat 0=L, 1=R
        State(int ml,int cl,int boat,int n){ this.ml=ml; this.cl=cl; this.boat=boat; this.n=n; }
        int mr(){ return n-ml; } int cr(){ return n-cl; }
        boolean valid(){
            boolean LOK = (ml==0 || ml>=cl), ROK = (mr()==0 || mr()>=cr());
            return ml>=0&&cl>=0&&mr()>=0&&cr()>=0&&LOK&&ROK;
        }
        boolean isGoal(){ return ml==0 && cl==0 && boat==1; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return ml==s.ml && cl==s.cl && boat==s.boat; }
        @Override public int hashCode(){ return Objects.hash(ml,cl,boat); }
        @Override public String toString(){ return "L("+ml+","+cl+")  R("+mr()+","+cr()+")  Boat:"+(boat==0?"L":"R"); }
    }
    static final int[][] MOV={{1,0},{2,0},{0,1},{0,2},{1,1}};

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();
    static Set<State> visiting=new HashSet<>();

    static boolean dls(State s, int limit){
        if(s.isGoal()) return true;
        if(limit==0) return false;
        visiting.add(s);
        int dir=s.boat==0?-1:1; String side=s.boat==0?"L->R":"R->L";
        for(int[] m: MOV){
            State t=new State(s.ml+dir*m[0], s.cl+dir*m[1], 1-s.boat, s.n);
            if(!t.valid() || visiting.contains(t)) continue;
            parent.put(t,s); action.put(t, side+" "+Arrays.toString(m));
            if(dls(t, limit-1)) return true;
        }
        visiting.remove(s);
        return false;
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal;x!=null;x=parent.get(x)){
            path.addFirst(x.equals(start)?x.toString():action.get(x)+" -> "+x);
        }
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Depth: "+(path.size()-1));
    }

    public static void main(String[] args){
        int n = args.length>0? Integer.parseInt(args[0]) : 3;
        int L = args.length>1? Integer.parseInt(args[1]) : 20;
        State st=new State(n,n,0,n);
        if(dls(st, L)){
            State g=null; for(State s: parent.keySet()) if(s.isGoal()){g=s;break;}
            printSolution("M&C DLS", st, g);
        }else{
            System.out.println("Not found within depth "+L);
        }
    }
}

PS16_EightQueens_DLS.java
import java.util.*;

/** PS16: Eight Queens — Depth-Limited Search (stand-alone). */
public class PS16_EightQueens_DLS {
    static final class State {
        final int n,next; final int[] colByRow;
        State(int n,int next,int[] c){ this.n=n; this.next=next; this.colByRow=c; }
        boolean isGoal(){ return next==n; }
        boolean safe(int r,int c){ for(int i=0;i<r;i++){ int v=colByRow[i]; if(v==c||Math.abs(v-c)==Math.abs(i-r)) return false; } return true; }
        State place(int r,int c){ int[] cp=colByRow.clone(); cp[r]=c; return new State(n,r+1,cp); }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return Arrays.equals(Arrays.copyOf(colByRow,next), Arrays.copyOf(s.colByRow,s.next)); }
        @Override public int hashCode(){ return Arrays.hashCode(Arrays.copyOf(colByRow,next)); }
        @Override public String toString(){ return Arrays.toString(Arrays.copyOf(colByRow,next)); }
    }

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();
    static Set<State> visiting=new HashSet<>();

    static boolean dls(State s, int limit){
        if(s.isGoal()) return true;
        if(limit==0) return false;
        visiting.add(s);
        int r=s.next;
        for(int c=0;c<s.n;c++){
            if(!s.safe(r,c)) continue;
            State t=s.place(r,c);
            if(visiting.contains(t)) continue;
            parent.put(t,s); action.put(t,"place r"+r+" c"+c);
            if(dls(t, limit-1)) return true;
        }
        visiting.remove(s);
        return false;
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal;x!=null;x=parent.get(x)) path.addFirst(x.equals(start)?x.toString():action.get(x)+" -> "+x);
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Depth: "+(path.size()-1));
    }

    public static void main(String[] args){
        int n = args.length>0? Integer.parseInt(args[0]) : 8;
        int L = args.length>1? Integer.parseInt(args[1]) : 8;
        State st=new State(n,0,new int[n]);
        if(dls(st, L)){
            State g=null; for(State s: parent.keySet()) if(s.isGoal()){g=s;break;}
            printSolution("Eight Queens DLS", st, g);
        }else{
            System.out.println("No solution within depth "+L);
        }
    }
}

PS18_EightPuzzle_BestFirst.java
import java.util.*;

/** PS18: 8-Puzzle — Greedy Best-First (Manhattan heuristic) stand-alone. */
public class PS18_EightPuzzle_BestFirst {
    static final class State {
        final int[] t; State(int[] t){ this.t=t; }
        boolean isGoal(){ return Arrays.equals(t, new int[]{1,2,3,4,5,6,7,8,0}); }
        int zero(){ for(int i=0;i<9;i++) if(t[i]==0) return i; return -1; }
        State swap(int i,int j){ int[] c=t.clone(); int v=c[i]; c[i]=c[j]; c[j]=v; return new State(c); }
        @Override public boolean equals(Object o){ return (o instanceof State s)&&Arrays.equals(t,s.t); }
        @Override public int hashCode(){ return Arrays.hashCode(t); }
        @Override public String toString(){ return Arrays.toString(t); }
    }
    static final int[][] MOV={{1,3},{0,2,4},{1,5},{0,4,6},{1,3,5,7},{2,4,8},{3,7},{4,6,8},{5,7}};
    static int h(State s){ int H=0; for(int i=0;i<9;i++){ int v=s.t[i]; if(v==0) continue; int gi=v-1; H+=Math.abs(i/3-gi/3)+Math.abs(i%3-gi%3);} return H; }

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();

    public static void main(String[] args){
        int[] start = args.length==9 ? Arrays.stream(args).mapToInt(Integer::parseInt).toArray()
                                     : new int[]{1,2,3,4,0,6,7,5,8};
        State st=new State(start);
        PriorityQueue<State> open = new PriorityQueue<>(Comparator.comparingInt(PS18_EightPuzzle_BestFirst::h));
        Set<State> closed=new HashSet<>();
        parent.put(st,null); open.add(st);

        while(!open.isEmpty()){
            State s=open.poll();
            if(s.isGoal()){ printSolution("8-Puzzle Greedy", st, s); return; }
            if(!closed.add(s)) continue;
            int z=s.zero();
            for(int m: MOV[z]){
                State t=s.swap(z,m);
                if(parent.containsKey(t)) continue;
                parent.put(t,s); action.put(t,"swap "+z+"-"+m);
                open.add(t);
            }
        }
        System.out.println("No solution (state may be unsolvable).");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal;x!=null;x=parent.get(x)) path.addFirst(x.equals(start)?x.toString():action.get(x)+" -> "+x);
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Steps: "+(path.size()-1));
    }
}

PS19_Maze_BestFirst.java
import java.util.*;

/** PS19: Maze — Greedy Best-First (Manhattan) stand-alone. */
public class PS19_Maze_BestFirst {
    static final class State {
        final int r,c,gr,gc; final boolean[][] w;
        State(int r,int c,boolean[][] w,int gr,int gc){ this.r=r; this.c=c; this.w=w; this.gr=gr; this.gc=gc; }
        boolean isGoal(){ return r==gr && c==gc; }
        boolean inside(int nr,int nc){ return nr>=0 && nc>=0 && nr<w.length && nc<w[0].length; }
        boolean free(int nr,int nc){ return !w[nr][nc]; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return r==s.r&&c==s.c; }
        @Override public int hashCode(){ return Objects.hash(r,c); }
        @Override public String toString(){ return "("+r+","+c+")"; }
    }
    static final int[] DR={-1,1,0,0};
    static final int[] DC={0,0,-1,1};
    static final String[] AN={"up","down","left","right"};
    static int h(State s){ return Math.abs(s.r - s.gr) + Math.abs(s.c - s.gc); }

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();

    public static void main(String[] args){
        int R=5,C=7; boolean[][] walls=new boolean[R][C];
        walls[1][1]=walls[1][2]=walls[1][3]=true; walls[3][3]=walls[3][4]=true;
        State st=new State(0,0,walls,4,6);

        PriorityQueue<State> open=new PriorityQueue<>(Comparator.comparingInt(PS19_Maze_BestFirst::h));
        Set<State> closed=new HashSet<>();
        open.add(st); parent.put(st,null);

        while(!open.isEmpty()){
            State s=open.poll();
            if(s.isGoal()){ printSolution("Maze Greedy", st, s); return; }
            if(!closed.add(s)) continue;
            for(int i=0;i<4;i++){
                int nr=s.r+DR[i], nc=s.c+DC[i];
                if(!s.inside(nr,nc)||!s.free(nr,nc)) continue;
                State t=new State(nr,nc,s.w,s.gr,s.gc);
                if(parent.containsKey(t)) continue;
                parent.put(t,s); action.put(t,AN[i]);
                open.add(t);
            }
        }
        System.out.println("No path.");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal;x!=null;x=parent.get(x)) path.addFirst(x.equals(start)?x.toString():action.get(x)+" -> "+x);
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Steps: "+(path.size()-1));
    }
}

PS20_MissionariesCannibals_BestFirst.java
import java.util.*;

/** PS20: Missionaries & Cannibals — Greedy Best-First (stand-alone). */
public class PS20_MissionariesCannibals_BestFirst {
    static final class State {
        final int ml,cl,n,boat; State(int ml,int cl,int boat,int n){ this.ml=ml; this.cl=cl; this.boat=boat; this.n=n; }
        int mr(){ return n-ml; } int cr(){ return n-cl; }
        boolean valid(){ boolean LOK=(ml==0||ml>=cl), ROK=(mr()==0||mr()>=cr()); return ml>=0&&cl>=0&&mr()>=0&&cr()>=0&&LOK&&ROK; }
        boolean isGoal(){ return ml==0&&cl==0&&boat==1; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return ml==s.ml&&cl==s.cl&&boat==s.boat; }
        @Override public int hashCode(){ return Objects.hash(ml,cl,boat); }
        @Override public String toString(){ return "L("+ml+","+cl+")  R("+mr()+","+cr()+")  Boat:"+(boat==0?"L":"R"); }
    }
    static final int[][] MOV={{1,0},{2,0},{0,1},{0,2},{1,1}};
    static int h(State s){ return s.ml + s.cl + (s.boat==0?1:0); }

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();

    public static void main(String[] args){
        int n=args.length>0?Integer.parseInt(args[0]):3;
        State st=new State(n,n,0,n);
        PriorityQueue<State> open=new PriorityQueue<>(Comparator.comparingInt(PS20_MissionariesCannibals_BestFirst::h));
        Set<State> closed=new HashSet<>();
        parent.put(st,null); open.add(st);

        while(!open.isEmpty()){
            State s=open.poll();
            if(s.isGoal()){ printSolution("M&C Greedy", st, s); return; }
            if(!closed.add(s)) continue;
            int dir=s.boat==0?-1:1; String side=s.boat==0?"L->R":"R->L";
            for(int[] m: MOV){
                State t=new State(s.ml+dir*m[0], s.cl+dir*m[1], 1-s.boat, s.n);
                if(!t.valid() || parent.containsKey(t)) continue;
                parent.put(t,s); action.put(t, side+" "+Arrays.toString(m));
                open.add(t);
            }
        }
        System.out.println("No solution.");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal;x!=null;x=parent.get(x)) path.addFirst(x.equals(start)?x.toString():action.get(x)+" -> "+x);
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Steps: "+(path.size()-1));
    }
}

PS21_EightQueens_BestFirst.java
import java.util.*;

/** PS21: Eight Queens — Greedy Best-First (stand-alone). */
public class PS21_EightQueens_BestFirst {
    static final class State {
        final int n,next; final int[] colByRow;
        State(int n,int next,int[] c){ this.n=n; this.next=next; this.colByRow=c; }
        boolean isGoal(){ return next==n; }
        boolean safe(int r,int c){ for(int i=0;i<r;i++){ int v=colByRow[i]; if(v==c||Math.abs(v-c)==Math.abs(i-r)) return false; } return true; }
        State place(int r,int c){ int[] cp=colByRow.clone(); cp[r]=c; return new State(n,r+1,cp); }
        static int h(State s){ return s.n - s.next; } // remaining rows (monotone for greedy tie-break)
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return Arrays.equals(Arrays.copyOf(colByRow,next), Arrays.copyOf(s.colByRow,s.next)); }
        @Override public int hashCode(){ return Arrays.hashCode(Arrays.copyOf(colByRow,next)); }
        @Override public String toString(){ return Arrays.toString(Arrays.copyOf(colByRow,next)); }
    }

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();

    public static void main(String[] args){
        int n=args.length>0?Integer.parseInt(args[0]):8;
        State st=new State(n,0,new int[n]);
        PriorityQueue<State> open=new PriorityQueue<>(Comparator.comparingInt(State::h));
        Set<State> closed=new HashSet<>();
        open.add(st); parent.put(st,null);

        while(!open.isEmpty()){
            State s=open.poll();
            if(s.isGoal()){ printSolution("Eight Queens Greedy", st, s); return; }
            if(!closed.add(s)) continue;
            int r=s.next;
            for(int c=0;c<n;c++){
                if(!s.safe(r,c)) continue;
                State t=s.place(r,c);
                if(parent.containsKey(t)) continue;
                parent.put(t,s); action.put(t,"place r"+r+" c"+c); open.add(t);
            }
        }
        System.out.println("No solution.");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal;x!=null;x=parent.get(x)) path.addFirst(x.equals(start)?x.toString():action.get(x)+" -> "+x);
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Steps: "+(path.size()-1));
    }
}

PS22_ShortestRoute_BestFirst.java
import java.util.*;

/** PS22: Shortest Route on Grid — Greedy Best-First (stand-alone). */
public class PS22_ShortestRoute_BestFirst {
    static final class State {
        final int r,c,gr,gc; final boolean[][] w;
        State(int r,int c,boolean[][] w,int gr,int gc){ this.r=r; this.c=c; this.w=w; this.gr=gr; this.gc=gc; }
        boolean isGoal(){ return r==gr && c==gc; }
        boolean inside(int nr,int nc){ return nr>=0 && nc>=0 && nr<w.length && nc<w[0].length; }
        boolean free(int nr,int nc){ return !w[nr][nc]; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return r==s.r&&c==s.c; }
        @Override public int hashCode(){ return Objects.hash(r,c); }
        @Override public String toString(){ return "("+r+","+c+")"; }
    }
    static final int[] DR={-1,1,0,0};
    static final int[] DC={0,0,-1,1};
    static final String[] AN={"up","down","left","right"};
    static int h(State s){ return Math.abs(s.r-s.gr)+Math.abs(s.c-s.gc); }

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();

    public static void main(String[] args){
        boolean[][] walls=new boolean[10][10];
        for(int i=2;i<9;i++) walls[i][5]=true;
        State st=new State(0,0,walls,9,9);

        PriorityQueue<State> open=new PriorityQueue<>(Comparator.comparingInt(PS22_ShortestRoute_BestFirst::h));
        Set<State> closed=new HashSet<>();
        open.add(st); parent.put(st,null);

        while(!open.isEmpty()){
            State s=open.poll();
            if(s.isGoal()){ printSolution("Shortest Route Greedy", st, s); return; }
            if(!closed.add(s)) continue;
            for(int i=0;i<4;i++){
                int nr=s.r+DR[i], nc=s.c+DC[i];
                if(!s.inside(nr,nc) || !s.free(nr,nc)) continue;
                State t=new State(nr,nc,s.w,s.gr,s.gc);
                if(parent.containsKey(t)) continue;
                parent.put(t,s); action.put(t,AN[i]); open.add(t);
            }
        }
        System.out.println("No path.");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal;x!=null;x=parent.get(x)) path.addFirst(x.equals(start)?x.toString():action.get(x)+" -> "+x);
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Steps: "+(path.size()-1));
    }
}

PS26_MissionariesCannibals_AStar.java
import java.util.*;

/** PS26: Missionaries & Cannibals — A* (stand-alone). */
public class PS26_MissionariesCannibals_AStar {
    static final class State {
        final int ml,cl,n,boat;
        State(int ml,int cl,int boat,int n){ this.ml=ml; this.cl=cl; this.boat=boat; this.n=n; }
        int mr(){ return n-ml; } int cr(){ return n-cl; }
        boolean valid(){ boolean LOK=(ml==0||ml>=cl), ROK=(mr()==0||mr()>=cr()); return ml>=0&&cl>=0&&mr()>=0&&cr()>=0&&LOK&&ROK; }
        boolean isGoal(){ return ml==0&&cl==0&&boat==1; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return ml==s.ml&&cl==s.cl&&boat==s.boat; }
        @Override public int hashCode(){ return Objects.hash(ml,cl,boat); }
        @Override public String toString(){ return "L("+ml+","+cl+")  R("+mr()+","+cr()+")  Boat:"+(boat==0?"L":"R"); }
    }
    static final int[][] MOV={{1,0},{2,0},{0,1},{0,2},{1,1}};
    static int h(State s){ return s.ml + s.cl + (s.boat==0?1:0); } // admissible-ish monotone proxy

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();
    static Map<State,Integer> g=new HashMap<>();

    public static void main(String[] args){
        int n=args.length>0?Integer.parseInt(args[0]):3;
        State st=new State(n,n,0,n);
        PriorityQueue<State> open=new PriorityQueue<>(Comparator.comparingInt(s->g.getOrDefault(s,0)+h(s)));
        g.put(st,0); parent.put(st,null); open.add(st);

        while(!open.isEmpty()){
            State s=open.poll();
            if(s.isGoal()){ printSolution("M&C A*", st, s); return; }
            int dir=s.boat==0?-1:1; String side=s.boat==0?"L->R":"R->L";
            for(int[] m: MOV){
                State t=new State(s.ml+dir*m[0], s.cl+dir*m[1], 1-s.boat, s.n);
                if(!t.valid()) continue;
                int ng=g.get(s)+1;
                if(ng < g.getOrDefault(t, Integer.MAX_VALUE)){
                    g.put(t,ng); parent.put(t,s); action.put(t, side+" "+Arrays.toString(m));
                    open.remove(t); open.add(t);
                }
            }
        }
        System.out.println("No solution.");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal;x!=null;x=parent.get(x)) path.addFirst(x.equals(start)?x.toString():action.get(x)+" -> "+x);
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Cost: "+(path.size()-1));
    }
}

PS27_EightQueens_AStar.java
import java.util.*;

/** PS27: Eight Queens — A* (stand-alone). */
public class PS27_EightQueens_AStar {
    static final class State {
        final int n,next; final int[] colByRow;
        State(int n,int next,int[] c){ this.n=n; this.next=next; this.colByRow=c; }
        boolean isGoal(){ return next==n; }
        boolean safe(int r,int c){ for(int i=0;i<r;i++){ int v=colByRow[i]; if(v==c||Math.abs(v-c)==Math.abs(i-r)) return false; } return true; }
        State place(int r,int c){ int[] cp=colByRow.clone(); cp[r]=c; return new State(n,r+1,cp); }
        static int h(State s){ return s.n - s.next; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return Arrays.equals(Arrays.copyOf(colByRow,next), Arrays.copyOf(s.colByRow,s.next)); }
        @Override public int hashCode(){ return Arrays.hashCode(Arrays.copyOf(colByRow,next)); }
        @Override public String toString(){ return Arrays.toString(Arrays.copyOf(colByRow,next)); }
    }

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();
    static Map<State,Integer> g=new HashMap<>();

    public static void main(String[] args){
        int n=args.length>0?Integer.parseInt(args[0]):8;
        State st=new State(n,0,new int[n]);
        PriorityQueue<State> open=new PriorityQueue<>(Comparator.comparingInt(s->g.getOrDefault(s,0)+State.h(s)));
        g.put(st,0); parent.put(st,null); open.add(st);

        while(!open.isEmpty()){
            State s=open.poll();
            if(s.isGoal()){ printSolution("Eight Queens A*", st, s); return; }
            int r=s.next;
            for(int c=0;c<n;c++){
                if(!s.safe(r,c)) continue;
                State t=s.place(r,c);
                int ng=g.get(s)+1;
                if(ng < g.getOrDefault(t,Integer.MAX_VALUE)){
                    g.put(t,ng); parent.put(t,s); action.put(t,"place r"+r+" c"+c);
                    open.remove(t); open.add(t);
                }
            }
        }
        System.out.println("No solution.");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal;x!=null;x=parent.get(x)) path.addFirst(x.equals(start)?x.toString():action.get(x)+" -> "+x);
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Cost: "+(path.size()-1));
    }
}

PS28_ShortestRoute_AStar.java
import java.util.*;

/** PS28: Shortest Route on Grid — A* (stand-alone). */
public class PS28_ShortestRoute_AStar {
    static final class State {
        final int r,c,gr,gc; final boolean[][] w;
        State(int r,int c,boolean[][] w,int gr,int gc){ this.r=r; this.c=c; this.w=w; this.gr=gr; this.gc=gc; }
        boolean isGoal(){ return r==gr && c==gc; }
        boolean inside(int nr,int nc){ return nr>=0 && nc>=0 && nr<w.length && nc<w[0].length; }
        boolean free(int nr,int nc){ return !w[nr][nc]; }
        @Override public boolean equals(Object o){ if(!(o instanceof State s)) return false; return r==s.r&&c==s.c; }
        @Override public int hashCode(){ return Objects.hash(r,c); }
        @Override public String toString(){ return "("+r+","+c+")"; }
    }
    static final int[] DR={-1,1,0,0};
    static final int[] DC={0,0,-1,1};
    static final String[] AN={"up","down","left","right"};
    static int h(State s){ return Math.abs(s.r-s.gr)+Math.abs(s.c-s.gc); }

    static Map<State,State> parent=new HashMap<>();
    static Map<State,String> action=new HashMap<>();
    static Map<State,Integer> g=new HashMap<>();

    public static void main(String[] args){
        boolean[][] walls=new boolean[12][12];
        for(int c=3;c<11;c++) walls[6][c]=true;
        for(int r=2;r<6;r++) walls[r][8]=true;

        State st=new State(0,0,walls,11,11);
        PriorityQueue<State> open=new PriorityQueue<>(Comparator.comparingInt(s->g.getOrDefault(s,0)+h(s)));
        g.put(st,0); parent.put(st,null); open.add(st);

        while(!open.isEmpty()){
            State s=open.poll();
            if(s.isGoal()){ printSolution("Shortest Route A*", st, s); return; }
            for(int i=0;i<4;i++){
                int nr=s.r+DR[i], nc=s.c+DC[i];
                if(!s.inside(nr,nc)||!s.free(nr,nc)) continue;
                State t=new State(nr,nc,s.w,s.gr,s.gc);
                int ng=g.get(s)+1;
                if(ng < g.getOrDefault(t,Integer.MAX_VALUE)){
                    g.put(t,ng); parent.put(t,s); action.put(t,AN[i]);
                    open.remove(t); open.add(t);
                }
            }
        }
        System.out.println("No path.");
    }

    static void printSolution(String title, State start, State goal){
        LinkedList<String> path=new LinkedList<>();
        for(State x=goal;x!=null;x=parent.get(x)) path.addFirst(x.equals(start)?x.toString():action.get(x)+" -> "+x);
        System.out.println("=== "+title+" ===");
        path.forEach(System.out::println);
        System.out.println("Cost: "+(path.size()-1));
    }
}

PS29_EightQueens_CSP.java
import java.util.*;

/** PS29: Eight Queens — simple CSP backtracking (stand-alone). */
public class PS29_EightQueens_CSP {
    static boolean ok(int[] asg, int upto){
        for(int i=0;i<upto;i++)
            for(int j=i+1;j<upto;j++)
                if(asg[i]==asg[j] || Math.abs(asg[i]-asg[j])==Math.abs(i-j))
                    return false;
        return true;
    }
    static boolean bt(int r, int[] asg){
        int n=asg.length; if(r==n) return true;
        for(int c=0;c<n;c++){ asg[r]=c; if(ok(asg,r+1) && bt(r+1,asg)) return true; }
        return false;
    }
    public static void main(String[] args){
        int n = args.length>0? Integer.parseInt(args[0]) : 8;
        int[] asg=new int[n];
        boolean ok=bt(0, asg);
        System.out.println("Solved: "+ok+" -> "+Arrays.toString(asg));
    }
}

PS30_Cryptarithmetic_SEND_MORE_MONEY.java
/** PS30: Cryptarithmetic — SEND + MORE = MONEY (stand-alone brute force with pruning). */
public class PS30_Cryptarithmetic_SEND_MORE_MONEY {
    public static void main(String[] args){
        outer:
        for(int s=1;s<=9;s++) for(int e=0;e<=9;e++) if(e!=s)
        for(int n=0;n<=9;n++) if(n!=s&&n!=e)
        for(int d=0; d<=9; d++) if(d!=s&&d!=e&&d!=n)
        for(int m=1;m<=9;m++) if(m!=s&&m!=e&&m!=n&&m!=d)
        for(int o=0;o<=9;o++) if(o!=s&&o!=e&&o!=n&&o!=d&&o!=m)
        for(int r=0;r<=9;r++) if(r!=s&&r!=e&&r!=n&&r!=d&&r!=m&&r!=o)
        for(int y=0;y<=9;y++) if(y!=s&&y!=e&&y!=n&&y!=d&&y!=m&&y!=o&&y!=r){
            int SEND=1000*s+100*e+10*n+d;
            int MORE=1000*m+100*o+10*r+e;
            int MONEY=10000*m+1000*o+100*n+10*e+y;
            if(SEND+MORE==MONEY){
                System.out.println("S="+s+" E="+e+" N="+n+" D="+d+" M="+m+" O="+o+" R="+r+" Y="+y);
                System.out.println(SEND+" + "+MORE+" = "+MONEY);
                break outer;
            }
        }
    }
}

PS31_GraphColoring_CSP.java
import java.util.*;

/** PS31: Graph Coloring (3 colors) CSP (stand-alone). */
public class PS31_GraphColoring_CSP {
    static Map<String,List<String>> adj = Map.of(
        "A", List.of("B","C"),
        "B", List.of("A","C","D"),
        "C", List.of("A","B","D"),
        "D", List.of("B","C")
    );
    static String[] colors={"Red","Green","Blue"};

    static boolean bt(List<String> vs, Map<String,String> asg, int i){
        if(i==vs.size()) return true;
        String v=vs.get(i);
        for(String c: colors){
            boolean ok=true;
            for(String n: adj.get(v)) if(c.equals(asg.get(n))){ ok=false; break; }
            if(ok){ asg.put(v,c); if(bt(vs,asg,i+1)) return true; asg.remove(v); }
        }
        return false;
    }

    public static void main(String[] args){
        var vs=new ArrayList<>(adj.keySet());
        var asg=new HashMap<String,String>();
        boolean solved=bt(vs,asg,0);
        System.out.println("Solved: "+solved+" -> "+asg);
    }
}

PS32_Predicate_FamilyTree.java
/** PS32: Predicate-style Family Tree demo (stand-alone). */
public class PS32_Predicate_FamilyTree {
    static class Person{ String n; Person f,m; Person(String n){this.n=n;} }
    static boolean isParent(Person p, Person c){ return c.f==p || c.m==p; }
    static boolean isGrandparent(Person gp, Person gc){ return (gc.f!=null&&isParent(gp,gc.f)) || (gc.m!=null&&isParent(gp,gc.m)); }

    public static void main(String[] args){
        Person john=new Person("John"), mary=new Person("Mary"), alice=new Person("Alice");
        alice.f=john; alice.m=mary;
        System.out.println("mother_of(Alice) = "+alice.m.n);
        System.out.println("father_of(Alice) = "+alice.f.n);
        System.out.println("is_parent(John,Alice) = "+isParent(john,alice));
        System.out.println("is_grandparent(John,Alice) = "+isGrandparent(john,alice));
    }
}

PS33_Predicate_BirdClassification.java
/** PS33: Predicate-style Bird classification demo (stand-alone). */
public class PS33_Predicate_BirdClassification {
    static String classify(boolean swims, boolean flies, boolean longLegs){
        if(swims && !flies) return "Penguin";
        if(flies && longLegs) return "Stork";
        if(flies) return "Sparrow";
        return "Unknown";
    }
    public static void main(String[] args){
        System.out.println(classify(true,false,false));  // Penguin
        System.out.println(classify(false,true,true));   // Stork
        System.out.println(classify(false,true,false));  // Sparrow
    }
}

PS34_Predicate_VehicleClassification.java
/** PS34: Predicate-style Vehicle classification (stand-alone). */
public class PS34_Predicate_VehicleClassification {
    static String classify(int wheels, boolean motor){
        if(wheels==2 && motor) return "Motorcycle";
        if(wheels==4 && motor) return "Car";
        if(wheels==4 && !motor) return "Cart";
        return "Unknown";
    }
    public static void main(String[] args){
        System.out.println(classify(2,true));
        System.out.println(classify(4,true));
        System.out.println(classify(4,false));
    }
}

PS35_Predicate_VegFruitClassification.java
/** PS35: Predicate-style Veg/Fruit classification (stand-alone). */
public class PS35_Predicate_VegFruitClassification {
    static String classify(boolean hasSeeds, boolean sweet){
        if(hasSeeds && sweet) return "Fruit";
        if(!hasSeeds && !sweet) return "Vegetable";
        return "Unknown";
    }
    public static void main(String[] args){
        System.out.println(classify(true,true));
        System.out.println(classify(false,false));
    }
}

PS36_Expert_SkillsetJob.java
import java.util.*;

/** PS36: Tiny Forward-Chaining Expert System — Skills -> Roles (stand-alone). */
public class PS36_Expert_SkillsetJob {
    static Set<String> infer(Set<String> facts, List<String[]> rules){
        Set<String> know=new HashSet<>(facts); boolean changed=true;
        while(changed){
            changed=false;
            for(String[] r: rules){
                // rule: IF r[0] AND r[1] THEN r[2]
                if(know.contains(r[0]) && know.contains(r[1]) && know.add(r[2])) changed=true;
            }
        }
        return know;
    }
    public static void main(String[] args){
        Set<String> facts=new HashSet<>(List.of("java","sql","ml"));
        List<String[]> rules=List.of(
            new String[]{"java","sql","backend_dev"},
            new String[]{"ml","python","data_scientist"}
        );
        System.out.println(infer(facts,rules));
    }
}

PS37_Expert_AnimalIdentification.java
import java.util.*;

/** PS37: Tiny Expert System — Animal identification (stand-alone). */
public class PS37_Expert_AnimalIdentification {
    static Set<String> infer(Set<String> facts, List<String[]> rules){
        Set<String> k=new HashSet<>(facts); boolean ch=true;
        while(ch){
            ch=false;
            for(String[] r: rules) if(k.contains(r[0]) && k.contains(r[1]) && k.add(r[2])) ch=true;
        }
        return k;
    }
    public static void main(String[] args){
        Set<String> facts=new HashSet<>(List.of("mammal","stripes","carnivore"));
        List<String[]> rules=List.of(
            new String[]{"mammal","stripes","tiger"},
            new String[]{"mammal","long_neck","giraffe"}
        );
        System.out.println(infer(facts,rules));
    }
}

PS38_Expert_FamilyTree.java
import java.util.*;

/** PS38: Expert-style Family facts (stand-alone demonstration). */
public class PS38_Expert_FamilyTree {
    public static void main(String[] args){
        Set<String> facts=new HashSet<>(List.of(
            "parent(john,alice)","parent(mary,alice)","male(john)","female(mary)","female(alice)"
        ));
        System.out.println("Facts: "+facts);
        System.out.println("Derived (conceptual): father(john,alice), mother(mary,alice)");
        System.out.println("Note: This toy demo does not implement term unification.");
    }
}

PS39_Expert_DiseaseClassification.java
import java.util.*;

/** PS39: Simple symptom -> disease rule base (stand-alone). */
public class PS39_Expert_DiseaseClassification {
    static Set<String> infer(Set<String> facts){
        Set<String> k=new HashSet<>(facts);
        if(k.contains("fever") && k.contains("cough")) k.add("flu");
        if(k.contains("fever") && k.contains("rash"))  k.add("measles");
        return k;
    }
    public static void main(String[] args){
        System.out.println(infer(new HashSet<>(List.of("fever","cough","fatigue"))));
    }
}

PS40_Expert_SentimentAnalysis.java
import java.util.*;

/** PS40: Token-based toy sentiment rules (stand-alone). */
public class PS40_Expert_SentimentAnalysis {
    static Set<String> toks(String s){ return new HashSet<>(Arrays.asList(s.toLowerCase().split("\\W+"))); }
    public static void main(String[] args){
        String text = (args.length>0) ? String.join(" ", args) : "I love this phone but battery life is poor";
        Set<String> facts=toks(text), out=new HashSet<>(facts);
        if(facts.contains("love") || facts.contains("excellent")) out.add("positive");
        if(facts.contains("poor") || facts.contains("bad"))       out.add("negative");
        System.out.println(out);
    }
}

Quick compile/run examples
# DFS Eight Queens
javac PS11_EightQueens_DFS.java && java PS11_EightQueens_DFS

# DLS Maze with custom depth
javac PS14_Maze_DLS.java && java PS14_Maze_DLS 20

# Greedy 8-puzzle with custom start
javac PS18_EightPuzzle_BestFirst.java && java PS18_EightPuzzle_BestFirst 1 2 3 4 0 6 7 5 8

# A* M&C for n=3
javac PS26_MissionariesCannibals_AStar.java && java PS26_MissionariesCannibals_AStar 3




PS32 – Predicate Family Tree

ps32_family_tree.pl

% Basic family predicates with derived relations.

male(john).
female(mary).
female(alice).
male(bob).

parent(john, alice).
parent(mary, alice).
parent(john, bob).
parent(mary, bob).

father(F,C) :- male(F), parent(F,C).
mother(M,C) :- female(M), parent(M,C).

grandparent(GP, GC) :- parent(GP, P), parent(P, GC).

sibling(X,Y) :-
    X \= Y,
    parent(P, X), parent(P, Y).

ancestor(A, D) :- parent(A, D).
ancestor(A, D) :- parent(A, X), ancestor(X, D).

/*
Queries:
?- father(john, alice).
?- mother(mary, bob).
?- grandparent(GP, alice).
?- sibling(alice, bob).
?- ancestor(john, alice).
*/

PS33 – Predicate Bird Classification

ps33_bird_classification.pl

% classify_bird(+Swims,+Flies,+LongLegs, -Species)

classify_bird(true,  false, _    , penguin).
classify_bird(true,  true,  _    , duck).
classify_bird(false, true,  true , stork).
classify_bird(false, true,  false, sparrow).
classify_bird(false, false, _    , flightless_unknown).

/*
Queries:
?- classify_bird(true,false,false,S).
S = penguin.

?- classify_bird(false,true,true,S).
S = stork.
*/

PS34 – Predicate Vehicle Classification

ps34_vehicle_classification.pl

% classify_vehicle(+Wheels, +Motor(boolean), -Type)

classify_vehicle(2, true,  motorcycle).
classify_vehicle(3, true,  auto_rickshaw).
classify_vehicle(4, true,  car).
classify_vehicle(4, false, cart).
classify_vehicle(W, false, bicycle) :- W =:= 2.
classify_vehicle(_, _, unknown).

/*
Queries:
?- classify_vehicle(2, true, T).
T = motorcycle.

?- classify_vehicle(4, false, T).
T = cart.
*/

PS35 – Predicate Veg/Fruit Classification

ps35_veg_fruit.pl

% classify_plant(+HasSeeds, +Sweet, -Type)

classify_plant(true,  true,  fruit).
classify_plant(false, false, vegetable).
classify_plant(true,  false, culinary_fruit_or_seeded_veg).
classify_plant(false, true,  processed_or_unknown).

/*
Queries:
?- classify_plant(true,true,T).
T = fruit.
*/

PS36 – Expert: Skillset → Job Role (forward-style reasoning)

ps36_skillset_job.pl

% Encode facts as a list of atoms (skills). infer_roles(+Skills, -AllFacts).
% Rules: IF java & sql THEN backend_dev. IF ml & python THEN data_scientist.

rule([java, sql], backend_dev).
rule([ml, python], data_scientist).
rule([frontend, react], frontend_dev).
rule([cloud, kubernetes], devops_engineer).

infer_roles(Skills, Inferred) :-
    sort(Skills, Base),         % normalize input
    closure(Base, Inferred).

closure(Facts, Out) :-
    ( apply_one_rule(Facts, NewAtom),
      \+ memberchk(NewAtom, Facts)
    -> closure([NewAtom|Facts], Out)
    ;  sort(Facts, Out)
    ).

apply_one_rule(Facts, Head) :-
    rule(Body, Head),
    forall(member(B, Body), memberchk(B, Facts)).

/*
Queries:
?- infer_roles([java,sql,ml,python], All).
All = [backend_dev, data_scientist, java, ml, python, sql].

?- infer_roles([cloud,kubernetes,react], All).
All = [cloud, devops_engineer, kubernetes, react].
*/

PS37 – Expert: Animal Identification

ps37_animal_ident.pl

% Very small rule base. infer_animal(+FactsList, -AllFacts).

% Rules:
rule([mammal, stripes, carnivore], tiger).
rule([mammal, long_neck]         , giraffe).
rule([bird, swims, not_flies]    , penguin).
rule([bird, flies, raptor]       , eagle).

infer_animal(Facts0, FactsOut) :-
    sort(Facts0, Base),
    closure(Base, FactsOut).

closure(F, Out) :-
    ( rule(Body, Head),
      subset_list(Body, F),
      \+ memberchk(Head, F)
    -> closure([Head|F], Out)
    ;  sort(F, Out)
    ).

subset_list([], _).
subset_list([H|T], set) :- !, false.  % guard if someone passes atom 'set'
subset_list([H|T], L) :- memberchk(H, L), subset_list(T, L).

/*
Queries:
?- infer_animal([mammal,stripes,carnivore], F).
F = [carnivore, mammal, stripes, tiger].

?- infer_animal([bird,swims,not_flies], F).
F = [bird, not_flies, penguin, swims].
*/

PS38 – Expert: Family Tree (facts + derived)

ps38_expert_family.pl

% Facts
male(john).
female(mary).
female(alice).

parent(john, alice).
parent(mary, alice).

% Derived "expert" rules
father(F,C) :- male(F), parent(F,C).
mother(M,C) :- female(M), parent(M,C).
child(C,P)  :- parent(P,C).

explain_father(C, F, proof(father(F,C), [male(F), parent(F,C)])) :-
    father(F,C).

explain_mother(C, M, proof(mother(M,C), [female(M), parent(M,C)])) :-
    mother(M,C).

/*
Queries:
?- father(F, alice).
F = john.

?- explain_father(alice, F, Proof).
F = john,
Proof = proof(father(john, alice), [male(john), parent(john, alice)]).
*/

PS39 – Expert: Disease Classification from Symptoms

ps39_disease_rules.pl

% infer_diseases(+SymptomsList, -AllFactsIncludingDiseases)

rule([fever, cough], flu).
rule([fever, rash] , measles).
rule([headache, nausea], migraine).

infer_diseases(Symptoms, Out) :-
    sort(Symptoms, Base),
    closure(Base, Out).

closure(F, Out) :-
    ( rule(Body, Head),
      subset_list(Body, F),
      \+ memberchk(Head, F)
    -> closure([Head|F], Out)
    ;  sort(F, Out)
    ).

subset_list([], _).
subset_list([H|T], L) :- memberchk(H, L), subset_list(T, L).

/*
Queries:
?- infer_diseases([fever,cough,fatigue], F).
F = [cough, fatigue, fever, flu].

?- infer_diseases([headache,nausea], F).
F = [headache, migraine, nausea].
*/

PS40 – Expert: Toy Sentiment Analysis

ps40_sentiment.pl

% classify_sentiment(+Tokens:list(atom), -Labels:list(atom))
% Labels include 'positive' and/or 'negative' if matched.

positive_word(love).
positive_word(excellent).
positive_word(good).
positive_word(amazing).

negative_word(poor).
negative_word(bad).
negative_word(terrible).
negative_word(horrible).

classify_sentiment(Tokens, Labels) :-
    (exists_positive(Tokens) -> P= [positive] ; P=[]),
    (exists_negative(Tokens) -> N= [negative] ; N=[]),
    append(P, N, Labels).

exists_positive(Toks) :- member(W,Toks), positive_word(W), !.
exists_negative(Toks) :- member(W,Toks), negative_word(W), !.

/*
Queries:
?- classify_sentiment([i,love,this,phone,but,battery,life,is,poor], L).
L = [positive, negative].

?- classify_sentiment([service,was,excellent], L).
L = [positive].
*/

