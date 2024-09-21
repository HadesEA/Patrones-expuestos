### 1. **Singleton**

- **Ejemplo del mundo real**: Supongamos que tienes una aplicación bancaria que solo debe tener una única instancia de la clase que gestiona las conexiones a la base de datos. Esta única instancia evita problemas como inconsistencias de datos o sobrecarga en la conexión.
  
- **Contexto**: El patrón Singleton asegura que haya una única instancia de esta clase para toda la aplicación y que pueda ser accesible globalmente.

- **Código en C#**:
```csharp
public class DatabaseConnection
{
    private static DatabaseConnection instance = null;
    private DatabaseConnection() 
    {
        Console.WriteLine("Conexión a la base de datos establecida.");
    }
    
    public static DatabaseConnection GetInstance()
    {
        if (instance == null)
        {
            instance = new DatabaseConnection();
        }
        return instance;
    }
}

public class Program
{
    public static void Main()
    {
        DatabaseConnection db1 = DatabaseConnection.GetInstance();
        DatabaseConnection db2 = DatabaseConnection.GetInstance();
        
        Console.WriteLine(ReferenceEquals(db1, db2)); // True, ambas referencias apuntan a la misma instancia.
    }
}
```

---

### 2. **Factory Method**

- **Ejemplo del mundo real**: En una aplicación de mensajería, puedes tener diferentes tipos de notificaciones (SMS, Email, Push). Dependiendo del contexto, la aplicación necesita crear instancias de diferentes tipos de notificaciones sin especificar directamente su clase.

- **Contexto**: El patrón Factory Method permite crear objetos de distintos tipos de notificaciones según el criterio, sin especificar la clase concreta que se va a instanciar.

- **Código en C#**:
```csharp
public abstract class Notification
{
    public abstract void Send();
}

public class SMSNotification : Notification
{
    public override void Send() 
    {
        Console.WriteLine("Enviando SMS...");
    }
}

public class EmailNotification : Notification
{
    public override void Send() 
    {
        Console.WriteLine("Enviando Email...");
    }
}

public abstract class NotificationFactory
{
    public abstract Notification CreateNotification();
}

public class SMSNotificationFactory : NotificationFactory
{
    public override Notification CreateNotification() 
    {
        return new SMSNotification();
    }
}

public class EmailNotificationFactory : NotificationFactory
{
    public override Notification CreateNotification() 
    {
        return new EmailNotification();
    }
}

public class Program
{
    public static void Main()
    {
        NotificationFactory factory = new SMSNotificationFactory();
        Notification notification = factory.CreateNotification();
        notification.Send(); // Enviando SMS...
    }
}
```

---

### 3. **Abstract Factory**

- **Ejemplo del mundo real**: Supón que estás creando una aplicación que tiene dos temas diferentes (oscuro y claro), y necesitas generar diferentes botones y textos de acuerdo con el tema seleccionado.

- **Contexto**: El patrón Abstract Factory ayuda a crear grupos de objetos relacionados (botón, texto) sin especificar sus clases concretas, asegurando que los objetos generados sean consistentes con el tema seleccionado.

- **Código en C#**:
```csharp
// Interfaces para productos
public interface IButton
{
    void Render();
}

public interface IText
{
    void Display();
}

// Implementaciones concretas para el tema oscuro
public class DarkButton : IButton
{
    public void Render() 
    {
        Console.WriteLine("Renderizando botón oscuro...");
    }
}

public class DarkText : IText
{
    public void Display() 
    {
        Console.WriteLine("Mostrando texto oscuro...");
    }
}

// Implementaciones concretas para el tema claro
public class LightButton : IButton
{
    public void Render() 
    {
        Console.WriteLine("Renderizando botón claro...");
    }
}

public class LightText : IText
{
    public void Display() 
    {
        Console.WriteLine("Mostrando texto claro...");
    }
}

// Abstract factory
public abstract class UIComponentFactory
{
    public abstract IButton CreateButton();
    public abstract IText CreateText();
}

// Factories concretas para cada tema
public class DarkThemeFactory : UIComponentFactory
{
    public override IButton CreateButton() 
    {
        return new DarkButton();
    }

    public override IText CreateText() 
    {
        return new DarkText();
    }
}

public class LightThemeFactory : UIComponentFactory
{
    public override IButton CreateButton() 
    {
        return new LightButton();
    }

    public override IText CreateText() 
    {
        return new LightText();
    }
}

public class Program
{
    public static void Main()
    {
        UIComponentFactory factory = new DarkThemeFactory();
        IButton button = factory.CreateButton();
        IText text = factory.CreateText();
        
        button.Render(); // Renderizando botón oscuro...
        text.Display();  // Mostrando texto oscuro...
    }
}
```

---

### 4. **Builder**

- **Ejemplo del mundo real**: Imagínate que necesitas construir diferentes configuraciones de computadoras. Las computadoras pueden tener distintas especificaciones como procesador, RAM, tarjeta gráfica, pero es importante seguir un proceso de construcción específico.

- **Contexto**: El patrón Builder permite construir objetos complejos paso a paso y permite reutilizar el proceso de construcción para distintas configuraciones.

- **Código en C#**:
```csharp
public class Computer
{
    public string CPU { get; set; }
    public string RAM { get; set; }
    public string GPU { get; set; }
    
    public override string ToString()
    {
        return $"CPU: {CPU}, RAM: {RAM}, GPU: {GPU}";
    }
}

public class ComputerBuilder
{
    private Computer computer = new Computer();
    
    public ComputerBuilder SetCPU(string cpu)
    {
        computer.CPU = cpu;
        return this;
    }
    
    public ComputerBuilder SetRAM(string ram)
    {
        computer.RAM = ram;
        return this;
    }
    
    public ComputerBuilder SetGPU(string gpu)
    {
        computer.GPU = gpu;
        return this;
    }
    
    public Computer Build()
    {
        return computer;
    }
}

public class Program
{
    public static void Main()
    {
        Computer gamingPC = new ComputerBuilder()
            .SetCPU("Intel i9")
            .SetRAM("32GB")
            .SetGPU("NVIDIA RTX 3080")
            .Build();
        
        Console.WriteLine(gamingPC); // CPU: Intel i9, RAM: 32GB, GPU: NVIDIA RTX 3080
    }
}
```

### 5. **Prototype**

- **Ejemplo del mundo real**: Supongamos que estás desarrollando un editor gráfico en el que los usuarios pueden clonar formas (círculos, cuadrados, etc.) para crear varias copias sin necesidad de crearlas desde cero. Al clonar, se copian las propiedades de la forma original.

- **Contexto**: El patrón Prototype permite crear nuevos objetos copiando una instancia existente, sin depender de la clase concreta de la que se originan.

- **Código en C#**:
```csharp
public abstract class Shape
{
    public string Color { get; set; }
    
    public abstract Shape Clone();
}

public class Circle : Shape
{
    public int Radius { get; set; }
    
    public override Shape Clone()
    {
        return (Shape)this.MemberwiseClone(); // Copia superficial
    }

    public override string ToString()
    {
        return $"Círculo con radio: {Radius}, color: {Color}";
    }
}

public class Program
{
    public static void Main()
    {
        Circle originalCircle = new Circle { Radius = 10, Color = "Rojo" };
        Circle clonedCircle = (Circle)originalCircle.Clone();
        
        Console.WriteLine(originalCircle);  // Círculo con radio: 10, color: Rojo
        Console.WriteLine(clonedCircle);   // Círculo con radio: 10, color: Rojo
    }
}
```

---

### 6. **Adapter**

- **Ejemplo del mundo real**: Imagina que estás desarrollando una aplicación de pago que debe integrarse con un sistema de pagos externo que no es compatible directamente con tu sistema. El patrón Adapter actúa como un "puente" entre tu sistema y el sistema de pagos, adaptando las interfaces.

- **Contexto**: El patrón Adapter convierte la interfaz de una clase en otra que un cliente espera, permitiendo que dos clases incompatibles trabajen juntas.

- **Código en C#**:
```csharp
// Sistema externo de pagos
public class ExternalPaymentSystem
{
    public void MakePayment()
    {
        Console.WriteLine("Pago realizado a través del sistema externo.");
    }
}

// Interfaz estándar de pagos en tu aplicación
public interface IPayment
{
    void Pay();
}

// Adapter que conecta el sistema externo con la interfaz de tu aplicación
public class PaymentAdapter : IPayment
{
    private ExternalPaymentSystem externalPaymentSystem;
    
    public PaymentAdapter(ExternalPaymentSystem system)
    {
        externalPaymentSystem = system;
    }
    
    public void Pay()
    {
        externalPaymentSystem.MakePayment(); // Adapta el método
    }
}

public class Program
{
    public static void Main()
    {
        ExternalPaymentSystem externalSystem = new ExternalPaymentSystem();
        IPayment payment = new PaymentAdapter(externalSystem);
        
        payment.Pay(); // Pago realizado a través del sistema externo.
    }
}
```

---

### 7. **Bridge**

- **Ejemplo del mundo real**: Supón que tienes una aplicación que maneja diferentes tipos de formas (círculo, cuadrado) y distintas formas de dibujarlas (en 2D o 3D). El patrón Bridge te permite separar la jerarquía de formas de la jerarquía de la representación (dibujar en 2D o 3D), permitiendo que ambas evolucionen independientemente.

- **Contexto**: El patrón Bridge desacopla una abstracción de su implementación, permitiendo que ambas varíen de manera independiente.

- **Código en C#**:
```csharp
// Implementación para dibujar en 2D o 3D
public interface IDrawAPI
{
    void Draw(string shape);
}

public class Draw2D : IDrawAPI
{
    public void Draw(string shape)
    {
        Console.WriteLine($"Dibujando {shape} en 2D.");
    }
}

public class Draw3D : IDrawAPI
{
    public void Draw(string shape)
    {
        Console.WriteLine($"Dibujando {shape} en 3D.");
    }
}

// Abstracción de las formas
public abstract class Shape
{
    protected IDrawAPI drawAPI;
    
    protected Shape(IDrawAPI drawAPI)
    {
        this.drawAPI = drawAPI;
    }
    
    public abstract void Draw();
}

public class Circle : Shape
{
    public Circle(IDrawAPI drawAPI) : base(drawAPI) {}
    
    public override void Draw()
    {
        drawAPI.Draw("círculo");
    }
}

public class Program
{
    public static void Main()
    {
        Shape circle2D = new Circle(new Draw2D());
        Shape circle3D = new Circle(new Draw3D());
        
        circle2D.Draw(); // Dibujando círculo en 2D.
        circle3D.Draw(); // Dibujando círculo en 3D.
    }
}
```

---

### 8. **Composite**

- **Ejemplo del mundo real**: Imagina que tienes un sistema que organiza archivos y carpetas. Una carpeta puede contener múltiples archivos o subcarpetas, y puedes realizar operaciones como "abrir" en cualquier elemento, sea un archivo o una carpeta.

- **Contexto**: El patrón Composite permite tratar objetos individuales (archivos) y compuestos (carpetas que contienen otros elementos) de manera uniforme.

- **Código en C#**:
```csharp
// Componente base (Archivo o Carpeta)
public interface IFileComponent
{
    void Open();
}

// Clase concreta Archivo
public class File : IFileComponent
{
    private string name;
    
    public File(string name)
    {
        this.name = name;
    }

    public void Open()
    {
        Console.WriteLine($"Abriendo archivo: {name}");
    }
}

// Clase concreta Carpeta
public class Folder : IFileComponent
{
    private string name;
    private List<IFileComponent> components = new List<IFileComponent>();
    
    public Folder(string name)
    {
        this.name = name;
    }

    public void Add(IFileComponent component)
    {
        components.Add(component);
    }

    public void Open()
    {
        Console.WriteLine($"Abriendo carpeta: {name}");
        foreach (var component in components)
        {
            component.Open();
        }
    }
}

public class Program
{
    public static void Main()
    {
        IFileComponent file1 = new File("Documento1.txt");
        IFileComponent file2 = new File("Documento2.txt");
        Folder folder = new Folder("MiCarpeta");
        
        folder.Add(file1);
        folder.Add(file2);
        
        folder.Open(); 
        // Abriendo carpeta: MiCarpeta
        // Abriendo archivo: Documento1.txt
        // Abriendo archivo: Documento2.txt
    }
}
```
