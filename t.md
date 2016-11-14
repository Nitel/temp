# A313 - NAHIM/LUCAS
## Exercice 1
**ecritCar :**
```java
void ecritCar(Character c) {
    System.out.print(c);
}
```
**ecritRectangle :**
```java
void ecritRectangle(int l, int L) {
    for (int i = 0; i < l; i++) {
        for (int j = 0; j < L; j++) {
            System.out.print('*');
        }
        System.out.print('\n');
    }
}
```
**ecritTriangle :**
```java
void ecritTriangle(int size) {
    int c = 0;
    for (int i = 0; i < size; i++) {
        c++;
        for (int _c = 0; _c < c; _c++) {
            System.out.print('*');
        }
        System.out.print('\n');
    }
}
```
**ecritTriangleCreux :**
```java
void ecritTriangleCreux(int size) {
    int c = 0;
    for (int i = 0; i < size; i++) {
        c++;
        for (int _c = 0; _c < c; _c++) {
            System.out.print(_c == 0 || _c == c-1 || i == size-1 ? '*' : ' ');
        }
        System.out.print('\n');
    }
}
```
**ecritTriangleDroite1 :**
```java
void ecritTriangleDroite1(int size) {
    for (int i = 0; i < size; i++) {
        for (int _i = 0; _i < i; _i++) {
            System.out.print(' ');
        }
        for (int _i = 0; _i < size-i; _i++) {
            System.out.print('*');
        }
        System.out.print('\n');
    }
}
```
**ecritTriangleDroite2 :**
```java
void ecritTriangleDroite2(int size) {
    for (int i = 0; i < size; i++) {
        for (int _i = 0; _i < size-i-1; _i++) {
            System.out.print(' ');
        }
        for (int _i = 0; _i < i+1; _i++) {
            System.out.print('*');
        }
        System.out.print('\n');
    }
}
```
## Exercice 2
**initial implementation**
```java
public class Echange {
    public void echange(int a, int b) {
        int temp = a;
        a = b;
        b = temp;
    }
}
public class EchangeTest extends junit.framework.TestCase {
    Echange e;
    Random rand;

    @Override
    public void setUp() throws Exception {
        super.setUp();
        rand = new Random();
        e = new Echange();
    }

    @Test
    public void testEchange() throws Exception {
        int _a = rand.nextInt(50), _b = rand.nextInt(50);
        int a = _a, b = _b;
        e.echange(a, b);
        Assert.assertEquals(_b, a);
        Assert.assertEquals(_a, b);
    }
}
```
**retour du test**
```bash
java.lang.AssertionError: 
Expected :25
Actual   :27
...
Process finished with exit code -1
```
Le test échoue en java car les paramètres ne sont pas passé par réference, ils sont passé par copie de valeur.
En remplacant le scalar "int" par l'object "Integer", la methode devrait passer les paramètres par réference. Cependant le test echoue à nouveau car la class Integer est un object immutable. un object immutable est un object qui ne peut être altéré.

**solution possible**
```java
public class Echange {
    public void echange(int[] a, int[] b) {
        int temp = a[0];
        a[0] = b[0];
        b[0] = temp;
    }
}
public class EchangeTest extends junit.framework.TestCase {
    Echange e;
    Random rand;

    @Override
    public void setUp() throws Exception {
        super.setUp();
        rand = new Random();
        e = new Echange();
    }

    @Test
    public void testEchange() throws Exception {
        int _a = rand.nextInt(50), _b = rand.nextInt(50);
        int[] a = new int[]{_a}, b = new int[]{_b};
        e.echange(a, b);
        Assert.assertEquals(_b, a[0]);
        Assert.assertEquals(_a, b[0]);
    }
}
```
**retour du test**
```bash
Process finished with exit code 0
```
## Exercice 3
**src**
```java
public class Calculs {
    public double distance(Point a, Point b) {
        return Math.sqrt(Math.pow(a.x - b.x, 2) + Math.pow(a.y - b.y, 2));
    }

    public int max(int a, int b, int c) {
        return Math.max(Math.max(a, b), c);
    }

    public Pair<Double, Double> div(int a, int b) {
        Double A = (double)a;
        Double B = (double)b;
        return new Pair<>(A/B, A%B);
    }
}
```
**tests**
```java
public class CalculsTest extends TestCase {
    Calculs c;

    @Override
    public void setUp() throws Exception {
        super.setUp();
        c = new Calculs();
    }

    @Test
    public void testDistance() throws Exception {
        Assert.assertEquals(2.828, c.distance(new Point(0, 0), new Point(2, 2)), 0.001);
    }

    @Test
    public void testMax() throws Exception {
        Assert.assertEquals(3, c.max(1, 2, 3));
    }

    @Test
    public void testDiv() throws Exception {
        Assert.assertEquals(new Pair<>(0.75, 3.0), c.div(3, 4));
    }
}
```
## Exercice 4
**src**
```java
public class Plan extends ArrayList<Point> {
    public Plan() {
        this(0);
    }

    public Plan(int n) {
        this(n, new Pair<>(-100, 100));
    }

    public Plan(int n, Pair<Integer, Integer> size) {
        super();
        Random rand = new Random();
        for (int i = 0; i<n; i++) {
            this.add(new Point(rand.nextInt(size.getValue()) + size.getKey(), rand.nextInt(size.getValue()) + size.getKey()));
        }
    }

    public Pair<Point, Point> closests() {
        Point p1 = null, p2 = null;
        for (Point _p1 : this) {
            if (p1 == null) {
                p1 = _p1;
            }
            for (Point _p2 : this) {
                if (_p2 == _p1) {
                    continue;
                }
                if (p2 == null) {
                    p2 = _p2;
                }
                if (_p1.distance(_p2) < p1.distance(p2)) {
                    p1 = _p1;
                    p2 = _p2;
                }
            }
        }

        return new Pair<>(p1, p2);
    }
}
```
**tests**
```java
public class PlanTest {

    @Test
    public void testNew() throws Exception {
        Plan plan = new Plan(1000);
        Assert.assertEquals(1000, plan.size());
        for (Point p : plan) {
            Assert.assertTrue(p.x >= -100 && p.x <= 100);
            Assert.assertTrue(p.y >= -100 && p.y <= 100);
        }
    }

    @Test
    public void testClosests() throws Exception {
        Plan plan = new Plan();
        Assert.assertEquals(new Pair<>(null, null), plan.closests());
        Point p1 = new Point(100, -100),
            p2 = new Point(49, -49),
            p3 = new Point(0, 0);
        plan.add(p1);
        plan.add(p2);
        plan.add(p3);
        Assert.assertEquals(new Pair<>(p2, p3), plan.closests());
    }
}
```
