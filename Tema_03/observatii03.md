1.  În OpenGL, ordinea de desenare a vârfurilor este, în mod implicit, în sens invers acelor de ceasornic. Dacă vârfurile sunt definite în această ordine, acea parte a formei va fi considerată "fața din față" a obiectului.
    private void DrawAxes()
    {

        //GL.LineWidth(3.0f);

        // Desenează axa Ox (cu roșu).
        GL.Begin(PrimitiveType.Lines);//inceputul functiei
        GL.Color3(Color.Red);
        GL.Vertex3(0, 0, 0);
        GL.Vertex3(XYZ_SIZE, 0, 0);
        //GL.End();

        // Desenează axa Oy (cu galben).
        //GL.Begin(PrimitiveType.Lines);
        GL.Color3(Color.Yellow);
        GL.Vertex3(0, 0, 0);
        GL.Vertex3(0, XYZ_SIZE, 0); ;
        // GL.End();

        // Desenează axa Oz (cu verde).
        //GL.Begin(PrimitiveType.Lines);
        GL.Color3(Color.Green);
        GL.Vertex3(0, 0, 0);
        GL.Vertex3(0, 0, XYZ_SIZE);
        GL.End();///finalul functiei

    }

2.Anti-aliasing este o tehnică utilizată în grafica computerizată care netizează marginile obiectelor grafice prin intermediul unor algoritmi care amestecă culorile pixelilor din jurul marginilor, creând astfel o tranziție mai lină între obiect și fundal. Totodată, îmbunătățește aspectul vizual al scenelor randate, făcând ca liniile și curbele să pară mai fluide și mai apropiate de realitate. Printre tipurile comune de anti-aliasing se numară SSAA, MSAA și FXAA.

3.GL.LineWidth(float) : Această comandă setează lățimea liniilor care vor fi desenate ulterior. Argumentul width este un număr cu virgulă mobilă (float) care specifică lățimea liniilor în pixeli.
GL.PointSize(float) : Această comandă setează dimensiunea punctelor care vor fi desenate. Parametrul size specifică diametrul punctelor în pixeli.
Aceste comenzi nu pot fi folosite în interiorul unui bloc GL.Begin() și GL.End(). Acestea sunt apeluri de configurare a stării OpenGL și trebuie executate în afara blocurilor GL.Begin()/GL.End().

5.  using OpenTK;
    using OpenTK.Graphics.OpenGL;
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

namespace lab3
{
internal class ImmediateMode:GameWindow
{

        public ImmediateMode():base(800,600)
        {
            VSync = VSyncMode.On;



        }
        protected override void OnLoad(EventArgs e)
        {
            base.OnLoad(e);
            GL.ClearColor(Color.Green);
            GL.Enable(EnableCap.DepthTest);
            GL.DepthFunc(DepthFunction.Less);



        }

        protected override void OnResize(EventArgs e)
        {
            base.OnResize(e);
            GL.Viewport(0, 0, Width / 2, Height);


        }

        protected override void OnUpdateFrame(FrameEventArgs e)
        {
            base.OnUpdateFrame(e);
        }

        protected override void OnRenderFrame(FrameEventArgs e)
        {
            base.OnRenderFrame(e);
        }


    }

}

4.LineLoop: Desenează o linie închisă prin conectarea ultimului vârf la primul.
LineStrip: Desenează o linie continuă, dar neînchisă,conectând fiecare vârf în ordine.
TriangleFan: Creează un evantai de triunghiuri împărtășind un vârf central comun.
TriangleStrip: Creează o secvență de triunghiuri conectate, fiecare triunghi împărtășind două vârfuri cu cel precedent.

6.  Utilizarea culorilor diferite (în gradient sau pe suprafețe) la desenarea obiectelor 3D este importantă pentru:
    -Îmbunătățirea percepției formei: Culorile și gradientele ajută la evidențierea contururilor și curburilor, făcând forma obiectului mai clară și mai ușor de înțeles.
    -Simularea iluminării: Diferitele culori sugerează cum ar cădea lumina pe suprafețe, oferind un aspect mai realist.
    -Vizualizarea detaliilor: Culorile diferite fac mai ușoară identificarea componentelor și a complexității obiectului.
    -Corectarea erorilor: Permite detectarea mai ușoară a problemelor de modelare, precum fațete lipsă sau orientări greșite.
    Avantaj: Oferă o vizibilitate mai bună și un realism sporit, făcând obiectele 3D mai ușor de interpretat.

7.Un gradient de culoare este o tranziție lină între două sau mai multe culori, aplicată pe o suprafață sau pe un obiect grafic. Acesta în OpenGL se obține prin specificarea culorilor diferite pentru vârfurile primitivei, iar OpenGL face interpolarea automată pentru a crea tranziția lină de culori.

8-9.using System;
using System.IO;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Input;

class TriangleApp : GameWindow
{
private Vector3[] triangleVertices;
private Color4[] vertexColors; // Culorile pentru fiecare vertex
private float cameraAngleX = 0.0f;
private float cameraAngleY = 0.0f;
private int selectedVertex = 0; // Vertexul selectat

    public TriangleApp(int width, int height, string title)
        : base(width, height, GraphicsMode.Default, title)
    {
        triangleVertices = LoadTriangleVertices("triangle_coords.txt");
        vertexColors = new Color4[3]
        {
            new Color4(1.0f, 0.0f, 0.0f, 1.0f), // Rosu
            new Color4(0.0f, 1.0f, 0.0f, 1.0f), // Verde
            new Color4(0.0f, 0.0f, 1.0f, 1.0f)  // Albastru
        };
    }

    private Vector3[] LoadTriangleVertices(string path)
    {
        string[] lines = File.ReadAllLines(path);
        Vector3[] vertices = new Vector3[3];
        for (int i = 0; i < 3; i++)
        {
            string[] parts = lines[i].Split(',');
            vertices[i] = new Vector3(
                float.Parse(parts[0]),
                float.Parse(parts[1]),
                float.Parse(parts[2])
            );
        }
        return vertices;
    }

    protected override void OnLoad(EventArgs e)
    {
        base.OnLoad(e);
        GL.ClearColor(0.0f, 0.0f, 0.0f, 1.0f); // Fundal negru
        GL.Enable(EnableCap.DepthTest);
        GL.Enable(EnableCap.Blend); // Activam blending-ul pentru transparenta
        GL.BlendFunc(BlendingFactor.SrcAlpha, BlendingFactor.OneMinusSrcAlpha); // Setam functia de blending
    }

    protected override void OnResize(EventArgs e)
    {
        base.OnResize(e);
        GL.Viewport(0, 0, Width, Height);
        GL.MatrixMode(MatrixMode.Projection);
        GL.LoadIdentity();
        Matrix4 perspective = Matrix4.CreatePerspectiveFieldOfView(MathHelper.DegreesToRadians(45.0f), Width / (float)Height, 0.1f, 100.0f);
        GL.LoadMatrix(ref perspective);
    }

    protected override void OnUpdateFrame(FrameEventArgs e)
    {
        base.OnUpdateFrame(e);

        // Schimbam vertexul selectat cu tastele 1, 2, 3 sau Keypad1, Keypad2, Keypad3
        KeyboardState keyboardState = Keyboard.GetState();

        if (keyboardState.IsKeyDown(Key.Number1) || keyboardState.IsKeyDown(Key.Keypad1))
        {
            selectedVertex = 0;
        }
        else if (keyboardState.IsKeyDown(Key.Number2) || keyboardState.IsKeyDown(Key.Keypad2))
        {
            selectedVertex = 1;
        }
        else if (keyboardState.IsKeyDown(Key.Number3) || keyboardState.IsKeyDown(Key.Keypad3))
        {
            selectedVertex = 2;
        }

        // Modificam valorile RGB pentru vertexul selectat
        if (keyboardState.IsKeyDown(Key.R))
            vertexColors[selectedVertex].R = MathHelper.Clamp(vertexColors[selectedVertex].R + 0.01f, 0.0f, 1.0f);
        if (keyboardState.IsKeyDown(Key.G))
            vertexColors[selectedVertex].G = MathHelper.Clamp(vertexColors[selectedVertex].G + 0.01f, 0.0f, 1.0f);
        if (keyboardState.IsKeyDown(Key.B))
            vertexColors[selectedVertex].B = MathHelper.Clamp(vertexColors[selectedVertex].B + 0.01f, 0.0f, 1.0f);

        // Afișarea valorilor RGB în consolă
        Console.Clear();
        for (int i = 0; i < vertexColors.Length; i++)
        {
            Console.WriteLine($"Vertex {i + 1} - R: {vertexColors[i].R:F2}, G: {vertexColors[i].G:F2}, B: {vertexColors[i].B:F2}");
        }

        // Controlul camerei cu mouse-ul
        if (Mouse.GetState().IsButtonDown(MouseButton.Left))
        {
            int deltaX = Mouse.GetState().X - Mouse.GetState().X;
            int deltaY = Mouse.GetState().Y - Mouse.GetState().Y;

            cameraAngleX += deltaX * 0.1f;
            cameraAngleY += deltaY * 0.1f;
        }
    }

    protected override void OnRenderFrame(FrameEventArgs e)
    {
        base.OnRenderFrame(e);

        GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
        GL.MatrixMode(MatrixMode.Modelview);
        GL.LoadIdentity();
        GL.Translate(0.0, 0.0, -5.0); // Mutam camera pe axa Z
        GL.Rotate(cameraAngleX, 0.0, 1.0, 0.0); // Rotim camera pe axa Y
        GL.Rotate(cameraAngleY, 1.0, 0.0, 0.0); // Rotim camera pe axa X

        GL.Begin(PrimitiveType.Triangles);

        // Aplicam culoarea specifica pentru fiecare vertex
        for (int i = 0; i < triangleVertices.Length; i++)
        {
            GL.Color4(vertexColors[i]);
            GL.Vertex3(triangleVertices[i]);
        }

        GL.End();

        SwapBuffers();
    }

    [STAThread]
    static void Main(string[] args)
    {
        using (TriangleApp app = new TriangleApp(800, 600, "OpenGL Triangle with Per-Vertex Color Control"))
        {
            app.Run(60.0);
        }
    }

}

// Concluzii:
// La apasarea tastelor 1, 2, sau 3, utilizatorul poate selecta vertexul corespunzator.
// Tastele R, G si B controleaza valorile canalelor de culoare pentru vertexul selectat.
// Valorile RGB pentru fiecare vertex sunt afisate in consola in timp real.

10.Utilizarea unei culori diferite pentru fiecare vârf în modurile LineStrip sau TriangleStrip creează un efect de gradient de culoare, deoarece OpenGL interpolează automat culorile între vârfuri, ceea ce îmbunătățește realismul și estetica obiectelor grafice.