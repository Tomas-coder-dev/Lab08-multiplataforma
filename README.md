import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'App Demo',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.teal,
        scaffoldBackgroundColor: Colors.grey.shade100,
        inputDecorationTheme: const InputDecorationTheme(
          border: OutlineInputBorder(),
        ),
      ),
      home: const LoginPage(),
    );
  }
}

class LoginPage extends StatelessWidget {
  const LoginPage({super.key});

  @override
  Widget build(BuildContext context) {
    final userController = TextEditingController();
    final passController = TextEditingController();

    return Scaffold(
      body: Center(
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(24.0),
          child: Card(
            shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(16)),
            elevation: 12,
            child: Padding(
              padding: const EdgeInsets.all(24.0),
              child: Column(
                mainAxisSize: MainAxisSize.min,
                children: [
                  Text('Iniciar Sesión', style: Theme.of(context).textTheme.headlineSmall),
                  const SizedBox(height: 20),
                  TextField(
                    controller: userController,
                    decoration: const InputDecoration(labelText: 'Usuario'),
                  ),
                  const SizedBox(height: 12),
                  TextField(
                    controller: passController,
                    obscureText: true,
                    decoration: const InputDecoration(labelText: 'Contraseña'),
                  ),
                  const SizedBox(height: 24),
                  SizedBox(
                    width: double.infinity,
                    child: ElevatedButton(
                      onPressed: () {
                        Navigator.pushReplacement(
                          context,
                          MaterialPageRoute(builder: (_) => const MainMenu()),
                        );
                      },
                      child: const Text('Ingresar'),
                    ),
                  )
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }
}

class MainMenu extends StatelessWidget {
  const MainMenu({super.key});

  @override
  Widget build(BuildContext context) {
    final options = [
      {'label': 'Ver FAQ', 'icon': Icons.question_answer, 'page': const FAQPage()},
      {'label': 'Ingresar Datos', 'icon': Icons.person, 'page': const DataForm()},
      {'label': 'Calculadora', 'icon': Icons.calculate, 'page': const CalculatorView()},
    ];

    return Scaffold(
      appBar: AppBar(
        title: const Text('Menú Principal'),
        actions: [
          IconButton(
            icon: const Icon(Icons.logout),
            tooltip: 'Cerrar Sesión',
            onPressed: () {
              Navigator.pushAndRemoveUntil(
                context,
                MaterialPageRoute(builder: (_) => const LoginPage()),
                (route) => false,
              );
            },
          )
        ],
      ),
      body: ListView.separated(
        padding: const EdgeInsets.all(24.0),
        itemCount: options.length,
        separatorBuilder: (_, __) => const SizedBox(height: 12),
        itemBuilder: (context, index) {
          final opt = options[index];
          return ElevatedButton.icon(
            style: ElevatedButton.styleFrom(
              padding: const EdgeInsets.symmetric(vertical: 18, horizontal: 24),
              shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
            ),
            icon: Icon(opt['icon'] as IconData),
            label: Text(opt['label'] as String, style: const TextStyle(fontSize: 18)),
            onPressed: () => Navigator.push(
              context,
              MaterialPageRoute(builder: (_) => opt['page'] as Widget),
            ),
          );
        },
      ),
    );
  }
}

class FAQPage extends StatelessWidget {
  const FAQPage({super.key});

  final List<FAQItem> faqList = const [
    FAQItem(question: '¿Cómo puedo cambiar mi contraseña?', answer: 'Ve a la sección de configuración y busca "Cambiar contraseña".'),
    FAQItem(question: '¿Cuáles son los métodos de pago aceptados?', answer: 'Tarjetas de crédito, transferencias y pagos en efectivo.'),
    FAQItem(question: '¿Cómo contacto con soporte técnico?', answer: 'Puedes escribirnos al correo soporte@ejemplo.com.'),
    FAQItem(question: '¿Puedo eliminar mi cuenta?', answer: 'Sí, en la configuración de tu perfil encontrarás la opción "Eliminar cuenta".'),
    FAQItem(question: '¿Cómo actualizo mi información personal?', answer: 'Dirígete a tu perfil y selecciona "Editar información".'),
    FAQItem(question: '¿Dónde puedo ver el historial de mis compras?', answer: 'En el menú principal, entra en "Mis pedidos".'),
    FAQItem(question: '¿La app funciona sin conexión?', answer: 'Algunas funciones están disponibles sin conexión.'),
    FAQItem(question: '¿Cómo recibo notificaciones importantes?', answer: 'Activa las notificaciones en tu configuración de usuario.'),
    FAQItem(question: '¿Se puede cambiar el idioma de la app?', answer: 'Sí, selecciona la opción "Idioma" en configuración.'),
    FAQItem(question: '¿Qué hago si olvidé mi contraseña?', answer: 'Usa la opción "¿Olvidaste tu contraseña?" en la pantalla de inicio de sesión.'),
    FAQItem(question: '¿Puedo acceder a la app desde varios dispositivos?', answer: 'Sí, solo necesitas iniciar sesión en cada dispositivo.'),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Preguntas Frecuentes')),
      body: ListView.builder(
        padding: const EdgeInsets.all(16),
        itemCount: faqList.length,
        itemBuilder: (context, index) {
          final faq = faqList[index];
          return Card(
            margin: const EdgeInsets.only(bottom: 12),
            child: ExpansionTile(
              title: Text(faq.question, style: const TextStyle(fontWeight: FontWeight.bold)),
              children: [
                Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Text(faq.answer),
                )
              ],
            ),
          );
        },
      ),
    );
  }
}

class FAQItem {
  final String question;
  final String answer;

  const FAQItem({required this.question, required this.answer});
}

class DataForm extends StatefulWidget {
  const DataForm({super.key});

  @override
  State<DataForm> createState() => _DataFormState();
}

class _DataFormState extends State<DataForm> {
  final _formKey = GlobalKey<FormState>();
  String nombre = '', apellido = '', dni = '';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Formulario')),
      body: Padding(
        padding: const EdgeInsets.all(24.0),
        child: Form(
          key: _formKey,
          child: ListView(
            children: [
              TextFormField(
                decoration: const InputDecoration(labelText: 'Nombre'),
                onSaved: (value) => nombre = value ?? '',
                validator: (value) => (value == null || value.isEmpty) ? 'Campo requerido' : null,
              ),
              const SizedBox(height: 12),
              TextFormField(
                decoration: const InputDecoration(labelText: 'Apellido'),
                onSaved: (value) => apellido = value ?? '',
                validator: (value) => (value == null || value.isEmpty) ? 'Campo requerido' : null,
              ),
              const SizedBox(height: 12),
              TextFormField(
                decoration: const InputDecoration(labelText: 'DNI'),
                keyboardType: TextInputType.number,
                onSaved: (value) => dni = value ?? '',
                validator: (value) => (value == null || value.isEmpty) ? 'Campo requerido' : null,
              ),
              const SizedBox(height: 24),
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: () {
                    if (_formKey.currentState!.validate()) {
                      _formKey.currentState!.save();
                      Navigator.pop(context);
                    }
                  },
                  child: const Text('Guardar'),
                ),
              )
            ],
          ),
        ),
      ),
    );
  }
}

class CalculatorView extends StatelessWidget {
  const CalculatorView({super.key});

  final List<String> buttons = const [
    '7', '8', '9', '/',
    '4', '5', '6', '*',
    '1', '2', '3', '-',
    '0', '.', '=', '+',
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Calculadora')),
      body: Padding(
        padding: const EdgeInsets.all(24.0),
        child: Column(
          children: [
            Container(
              alignment: Alignment.centerRight,
              padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
              height: 80,
              decoration: BoxDecoration(
                color: Colors.white,
                borderRadius: BorderRadius.circular(12),
                border: Border.all(color: Colors.teal.shade100),
              ),
              child: const Text('0', style: TextStyle(fontSize: 32, fontWeight: FontWeight.bold)),
            ),
            const SizedBox(height: 20),
            Expanded(
              child: GridView.builder(
                itemCount: buttons.length,
                gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
                  crossAxisCount: 4,
                  crossAxisSpacing: 12,
                  mainAxisSpacing: 12,
                ),
                itemBuilder: (context, index) {
                  final text = buttons[index];
                  return ElevatedButton(
                    onPressed: () {},
                    style: ElevatedButton.styleFrom(
                      shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(10)),
                    ),
                    child: Text(text, style: const TextStyle(fontSize: 20)),
                  );
                },
              ),
            )
          ],
        ),
      ),
    );
  }
}
