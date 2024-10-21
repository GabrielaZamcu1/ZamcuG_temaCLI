1. GL.Ortho(-5.0, 5.0, -5.0, 1.0, 0.0, 4.0);

Observații: Această comandă mută triunghiul în partea de sus și îl redimensionează. Volumul de vizualizare este definit de margini între -5.0 și 5.0 pe axa X, între -5.0 și 1.0 pe axa Y, și între 0.0 și 4.0 pe axa Z.


3.

1 - Viewport-ul este zona din fereastră în care se afișează conținutul grafic, și este setată cu glViewport(x, y, width, height). Aceasta definește porțiunea din fereastră unde se desenează scena.

2 - FPS (Frames per Second) indică numărul de cadre afișate într-o secundă. O valoare mai mare a FPS-ului asigură o animație mai cursivă și îmbunătățește performanța generală a aplicației.

3 - OnUpdateFrame() se ocupă de actualizarea logicii aplicației în fiecare cadru, inclusiv gestionarea mișcării obiectelor și răspsunsul la intrările de la utilizator.

4 - Immediate Mode Rendering în OpenGL permite desenarea obiectelor cadru cu cadru în timp real, dar este mai puțin eficient decât tehnicile moderne folosite pentru performanță ridicată.

5 - OpenGL 3.3 este ultima versiune care acceptă modul de randare imediată, înlocuit ulterior cu metode mai eficiente, cum ar fi Vertex Buffer Objects (VBO).

6 - OnRenderFrame() gestionează randarea scenei după ce logica aplicației a fost actualizată, responsabilă de afișarea conținutului grafic.

7 - OnResize() ajustează viewport-ul și proiecțiile atunci când dimensiunea ferestrei se schimbă, pentru a păstra corect proporțiile grafice.

8 - CreatePerspectiveFieldOfView() creează o matrice de proiecție în perspectivă, setând unghiul de vizualizare, raportul de aspect și planurile apropiat/îndepărtat, pentru o reprezentare corectă a obiectelor într-o scenă 3D.
