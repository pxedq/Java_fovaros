# Java_fovaros
```
import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.Scanner;
import java.util.TreeMap;
import java.util.TreeSet;

public class Main {
    private class Fovaros {
        public String orszag;
        public String rov;
        public int lakos;
        public String fovaros;
        public int folakos;

        public Fovaros(String sor) {
            String[] s = sor.split(";");
            orszag = s[0];
            rov = s[1];
            lakos = Integer.parseInt(s[2]);
            fovaros = s[3];
            folakos = Integer.parseInt(s[4]);
        }
    }

    private ArrayList<Fovaros> fovarosok = new ArrayList<>();

    public Main() {
        // --- 0. feladat ---
        betolt("fovaros.csv");
        System.out.printf("0) Összesen %d ország adata lett eltárolva.",fovarosok.size());

        // --- 1. feladat ---
        Fovaros leg = fovarosok.get(0);
        for (Fovaros f : fovarosok) {
            if (f.lakos > leg.lakos) {
                leg = f;
            }
        }
        Fovaros leg2 = null;
        int lak2 = 0;
        for (Fovaros f : fovarosok) {
            if (f.lakos > lak2 && f.lakos < leg.lakos) {
                leg2 = f;
                lak2 = f.lakos;
            }
        }
        System.out.printf("1) Az ország, ahol a legtöbben élnek: %s, %,d fő\n", leg.orszag, leg.lakos);
        System.out.printf("   A második legnagyobb népesség: %s, %,d fő˛\n", leg2.orszag, leg2.lakos);

        // --- 2. feladat ---
        int i = 0;
        while (!fovarosok.get(i).fovaros.equals("Budapest")) {
            i++;
        }
        Fovaros Bp = fovarosok.get(i);
        int db = 0;
        for (Fovaros f : fovarosok) {
            if (f.folakos < Bp.folakos) {
                db++;
            }
        }
        System.out.printf("2) Összesen %d fővárosban élnek kevesebben, mint Budapesten!", db);

        // --- 3. feladat ---
        TreeSet<String> rovek = new TreeSet<>();
        for (Fovaros f : fovarosok) {
            if (f.rov.contains("C")) {
                rovek.add(f.rov);
            }
        }
        System.out.printf("3) Országjel, amiben szerepel 'C' betű: %s\n", String.join(", ", rovek));

        // --- 4. feladat ---
        int osszes = 0;
        for (Fovaros f : fovarosok) {
            if (f.lakos < 20_000_000) {
                osszes += f.lakos;
            }
        }
        System.out.printf("4) A 20 millió főnél kisebb országok fővárosainak össznépessége. %,d fő\n", osszes);

        // --- 5. feladat ---
        TreeMap<Integer, Integer> stat = new TreeMap<>();
        for (Fovaros f : fovarosok) {
            int kat = f.folakos / 5_000_000;
            if (!stat.containsKey(kat)) {
                stat.put(kat, 1);
            } else {
                stat.put(kat, stat.get(kat) + 1);
            }
        }
        System.out.printf("5) Fővárosok népesség szerint csoportosítva (5 milló fő):\n");
        for (Integer kat : stat.keySet()) {
            System.out.printf("   %,10d - %,10d: %d\n", kat*5_000_000, (kat+1)*5_000_000-1, stat.get(kat));
        }

        // --- 6. feladat ---
        PrintWriter ki = null;
        try {
            ki = new PrintWriter(new File("nagyok.txt"), "utf-8");
            for (Fovaros f : fovarosok) {
                if (f.lakos > 200_000_000) {
                    ki.printf("%s, %,d\r\n", f.orszag, f.lakos);
                }
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            if (ki != null) {
                ki.close();
            }
        }
        System.out.printf("6) Nagy népességű országok a nagyok.txt fájlban!\n");
    }

    private void betolt(String fajlnev) {
        Scanner be = null;
        try {
            be = new Scanner(new File(fajlnev), "utf-8");
            be.nextLine();
            while (be.hasNextLine()) {
                fovarosok.add(new Fovaros(be.nextLine()));
            }
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } finally {
            if (be != null) {
                be.close();
            }
        }
    }

    public static void main(String[] args) {
        new Main();
    }
}
```
### fovaros.csv:
```
Ország;Rövidítés;Ország lakossága;Főváros;Főváros lakossága
Albánia;AL;2900000;Tirana;800000
Algeria;DZ;44200000;Algiers;3400000
Andorra;AD;80000;Andorra la Vella;20000
...
```
