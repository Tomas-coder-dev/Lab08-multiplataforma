import 'package:flutter/material.dart';
import 'package:flutter/services.dart'; // Import for input formatters
import 'dart:math'; // Import for round()

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'App Demo Épico',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        // Use a darker shade of teal as the primary color for a richer feel
        primarySwatch: Colors.teal,
        primaryColor: Colors.teal.shade700,
        // A slightly darker background for better contrast and a modern look
        scaffoldBackgroundColor: Colors.blueGrey.shade900,
        brightness: Brightness.dark, // Enable dark mode styling
        textTheme: const TextTheme(
          // Enhance headline style
          headlineSmall: TextStyle(
              color: Colors.white,
              fontSize: 28,
              fontWeight: FontWeight.bold,
              letterSpacing: 1.2),
          // Improve body text style
          bodyMedium: TextStyle(color: Colors.white70, fontSize: 16),
          labelLarge: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
        ),
        cardTheme: CardTheme(
          color: Colors.blueGrey.shade800, // Darker card background
          elevation: 16, // More pronounced shadow
          shape:
              RoundedRectangleBorder(borderRadius: BorderRadius.circular(20)), // More rounded corners
          margin: EdgeInsets.zero, // Remove default card margin for more control
        ),
        inputDecorationTheme: InputDecorationTheme(
          filled: true, // Add a filled background to input fields
          fillColor: Colors.blueGrey.shade700, // Slightly lighter fill color
          border: OutlineInputBorder(
            borderRadius: BorderRadius.circular(12), // Rounded input borders
            borderSide: BorderSide.none, // No visible border line
          ),
          focusedBorder: OutlineInputBorder(
            borderRadius: BorderRadius.circular(12),
            borderSide: BorderSide(
                color: Colors.tealAccent.shade400, width: 2), // Highlight focused border
          ),
          labelStyle: TextStyle(color: Colors.white60), // Lighter label color
          hintStyle: TextStyle(color: Colors.white30), // Lighter hint color
          contentPadding:
              const EdgeInsets.symmetric(horizontal: 20, vertical: 18), // Add padding
        ),
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            foregroundColor: Colors.white, // White text on button
            backgroundColor: Colors.tealAccent.shade700, // Vibrant button color
            padding: const EdgeInsets.symmetric(vertical: 18, horizontal: 24),
            shape: RoundedRectangleBorder(
                borderRadius: BorderRadius.circular(12)), // Rounded button corners
            textStyle: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            elevation: 8, // Add button shadow
          ),
        ),
        appBarTheme: AppBarTheme(
          backgroundColor: Colors.blueGrey.shade900, // Dark app bar
          foregroundColor: Colors.white, // White icons and text
          elevation: 10, // App bar shadow
          centerTitle: true, // Center app bar title
          titleTextStyle: const TextStyle(
              color: Colors.white, fontSize: 24, fontWeight: FontWeight.bold),
        ),
        // Add a distinct color for accents like icons or progress indicators
        colorScheme: ColorScheme.dark(
          primary: Colors.tealAccent.shade700,
          secondary: Colors.cyanAccent.shade400,
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

    // Dispose controllers when the widget is removed from the tree
    // Note: For a simple StatelessWidget, disposing here is not strictly necessary
    // as they will be garbage collected, but it's good practice in State-ful widgets.
    // In a larger app, you might manage controllers with a state management solution.
    // For this example, we'll keep them here for simplicity but acknowledge
    // that in a real app with complex state, a StatefulWidget and dispose() would be better.

    return Scaffold(
      body: Center(
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(32.0), // Increased padding
          child: Card(
            child: Padding(
              padding: const EdgeInsets.all(32.0), // Increased padding inside card
              child: Column(
                mainAxisSize: MainAxisSize.min,
                children: [
                  Text('Acceso Épico',
                      style: Theme.of(context).textTheme.headlineSmall), // Updated title
                  const SizedBox(height: 40), // Increased spacing
                  TextField(
                    controller: userController,
                    decoration: const InputDecoration(
                      labelText: 'Usuario Épico', // Updated label
                      prefixIcon: Icon(Icons.person_outline), // Add icon
                    ),
                  ),
                  const SizedBox(height: 20), // Increased spacing
                  TextField(
                    controller: passController,
                    obscureText: true,
                    decoration: const InputDecoration(
                      labelText: 'Contraseña Ultra Secreta', // Updated label
                      prefixIcon: Icon(Icons.lock_outline), // Add icon
                    ),
                  ),
                  const SizedBox(height: 40), // Increased spacing
                  SizedBox(
                    width: double.infinity,
                    child: ElevatedButton(
                      onPressed: () {
                        // In a real app, validate credentials here
                        Navigator.pushReplacement(
                          context,
                          MaterialPageRoute(builder: (_) => const MainMenu()),
                        );
                      },
                      child: const Text('Ingresar al Reino'), // Updated button text
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
      {'label': 'Saber del Universo', 'icon': Icons.question_answer, 'page': const FAQPage()},
      {'label': 'Registrar Héroe', 'icon': Icons.person_add, 'page': const DataForm()}, // Updated icon
      {'label': 'Máquina de Cálculo', 'icon': Icons.calculate, 'page': const CalculatorView()},
    ];

    return Scaffold(
      appBar: AppBar(
        title: const Text('Nexus Principal'), // Updated title
        actions: [
          IconButton(
            icon: const Icon(Icons.logout),
            tooltip: 'Abandonar Sesión', // Updated tooltip
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
        padding: const EdgeInsets.all(24.0), // Added padding
        itemCount: options.length,
        separatorBuilder: (_, __) => const SizedBox(height: 16), // Increased spacing
        itemBuilder: (context, index) {
          final opt = options[index];
          return ElevatedButton.icon(
            style: ElevatedButton.styleFrom(
              padding: const EdgeInsets.symmetric(vertical: 20, horizontal: 24), // Increased padding
              shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(16)), // More rounded corners
              // You could add a gradient here for extra epicness
              // primary: Colors.tealAccent.shade700,
              // onPrimary: Colors.white,
            ),
            icon: Icon(opt['icon'] as IconData, size: 28), // Increased icon size
            label: Text(opt['label'] as String,
                style: const TextStyle(fontSize: 20)), // Increased text size
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
    FAQItem(
        question: '¿Cómo alterar mi esencia?',
        answer: 'Dirígete al Crisol de Transformación y busca "Modificar Esencia".'), // Updated text
    FAQItem(
        question: '¿Qué tributos son aceptados?',
        answer: 'Cristales de energía, gemas raras y ofrendas ancestrales.'), // Updated text
    FAQItem(
        question: '¿Cómo invoco a los guardianes de soporte?',
        answer: 'Puedes enviar un mensaje etéreo a guardianes@reinoepico.com.'), // Updated text
    FAQItem(
        question: '¿Puedo disolver mi forma en el éter?',
        answer:
            'Sí, en los Archivos Akáshicos de tu perfil encontrarás la opción "Disolver Forma".'), // Updated text
    FAQItem(
        question: '¿Cómo grabo mis proezas recientes?',
        answer: 'Dirígete a tus Crónicas de Héroe y selecciona "Registrar Proeza".'), // Updated text
    FAQItem(
        question: '¿Dónde contemplo mis victorias pasadas?',
        answer: 'En el Nexus Principal, entra en "Crónicas de Batalla".'), // Updated text
    FAQItem(
        question: '¿Funciona el oráculo sin conexión al velo?',
        answer: 'Algunas visiones están disponibles sin conexión al velo.'), // Updated text
    FAQItem(
        question: '¿Cómo recibo presagios importantes?',
        answer: 'Activa los presagios en tu configuración de alma.'), // Updated text
    FAQItem(
        question: '¿Se puede alterar el dialecto del oráculo?',
        answer: 'Sí, selecciona la opción "Dialecto" en configuración.'), // Updated text
    FAQItem(
        question: '¿Qué hago si mi clave rúnica fue olvidada?',
        answer:
            'Usa la opción "¿Clave rúnica olvidada?" en el portal de acceso.'), // Updated text
    FAQItem(
        question: '¿Puedo acceder al reino desde múltiples planos?',
        answer: 'Sí, solo necesitas iniciar sesión en cada plano de existencia.'), // Updated text
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Sabiduría Ancestral')), // Updated title
      body: ListView.builder(
        padding: const EdgeInsets.all(16),
        itemCount: faqList.length,
        itemBuilder: (context, index) {
          final faq = faqList[index];
          return Card(
            margin: const EdgeInsets.only(bottom: 16), // Increased margin
            child: ExpansionTile(
              collapsedIconColor: Colors.white70, // Icon color when collapsed
              iconColor: Colors.tealAccent, // Icon color when expanded
              title: Text(faq.question,
                  style: const TextStyle(fontWeight: FontWeight.bold, color: Colors.white)), // Title color
              children: [
                Padding(
                  padding: const EdgeInsets.all(16.0)
                      .copyWith(top: 0), // Adjust padding
                  child: Text(faq.answer,
                      style: Theme.of(context).textTheme.bodyMedium), // Use bodyMedium style
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
  final TextEditingController nombreController = TextEditingController();
  final TextEditingController apellidoController = TextEditingController();
  final TextEditingController dniController = TextEditingController();

  @override
  void dispose() {
    // Dispose controllers when the widget is removed
    nombreController.dispose();
    apellidoController.dispose();
    dniController.dispose();
    super.dispose();
  }

  void _saveForm() {
    if (_formKey.currentState!.validate()) {
      _formKey.currentState!.save();
      // Process the saved data (nombre, apellido, dni)
      print('Datos guardados:');
      // Using string interpolation as suggested
      print('Nombre: ${nombreController.text}');
      print('Apellido: ${apellidoController.text}');
      print('DNI: ${dniController.text}');

      // Show a confirmation SnackBar
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('¡Héroe registrado con éxito!', style: TextStyle(color: Colors.white)),
          backgroundColor: Colors.green, // Success color
          behavior: SnackBarBehavior.floating, // Make it float
          duration: Duration(seconds: 3), // How long it's visible
        ),
      );

      // Optionally navigate back after saving
      // Navigator.pop(context);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Registro de Héroe')), // Updated title
      body: Padding(
        padding: const EdgeInsets.all(24.0), // Added padding
        child: Form(
          key: _formKey,
          child: ListView(
            children: [
              TextFormField(
                controller: nombreController,
                decoration: const InputDecoration(
                  labelText: 'Nombre de Héroe', // Updated label
                  prefixIcon: Icon(Icons.badge), // Add icon
                ),
                onSaved: (value) {}, // Data is directly in controller
                validator: (value) =>
                    (value == null || value.isEmpty) ? 'El nombre es crucial' : null, // Updated validation text
              ),
              const SizedBox(height: 16), // Increased spacing
              TextFormField(
                controller: apellidoController,
                decoration: const InputDecoration(
                  labelText: 'Linaje de Héroe', // Updated label
                  prefixIcon: Icon(Icons.family_restroom), // Add icon
                ),
                onSaved: (value) {}, // Data is directly in controller
                validator: (value) => (value == null || value.isEmpty)
                    ? 'El linaje es importante'
                    : null, // Updated validation text
              ),
              const SizedBox(height: 16), // Increased spacing
              TextFormField(
                controller: dniController,
                decoration: const InputDecoration(
                  labelText: 'Marca de Identidad', // Updated label
                  prefixIcon: Icon(Icons.fingerprint), // Add icon
                ),
                keyboardType: TextInputType.number,
                inputFormatters: [FilteringTextInputFormatter.digitsOnly], // Only allow digits
                onSaved: (value) {}, // Data is directly in controller
                validator: (value) => (value == null || value.isEmpty)
                    ? 'La marca de identidad es necesaria'
                    : null, // Updated validation text
              ),
              const SizedBox(height: 32), // Increased spacing
              SizedBox(
                width: double.infinity,
                child: ElevatedButton.icon(
                  icon: const Icon(Icons.save), // Add icon to button
                  label: const Text('Forjar Registro'), // Updated button text
                  onPressed: _saveForm, // Call the save method
                  style: ElevatedButton.styleFrom(
                      padding: const EdgeInsets.symmetric(vertical: 18)), // Adjust padding
                ),
              )
            ],
          ),
        ),
      ),
    );
  }
}

class CalculatorView extends StatefulWidget {
  const CalculatorView({super.key});

  @override
  State<CalculatorView> createState() => _CalculatorViewState();
}

class _CalculatorViewState extends State<CalculatorView> {
  String _output = "0";
  String _input = "";
  double num1 = 0.0;
  double num2 = 0.0;
  String operator = "";

  void _buttonPressedRevised(String buttonText) {
    if (buttonText == "AC") {
      _output = "0";
      _input = "";
      num1 = 0.0;
      num2 = 0.0;
      operator = "";
    } else if (buttonText == "DEL") {
      if (_input.isNotEmpty) {
        _input = _input.substring(0, _input.length - 1);
        // Attempt to update output based on remaining input, basic handling
        if (_input.isEmpty || (_input.isNotEmpty && _isOperator(_input.substring(_input.length - 1)))) {
          _output = "0";
        } else {
          // This is a simplified approach; a proper parser would be better
          try {
            // Split by any operator and take the last part
            List<String> parts = _input.split(RegExp(r'[+\-*/]'));
            String lastPart = parts.last;
            if (lastPart.isEmpty && parts.length > 1) {
               // If the last part is empty, it means input ends with an operator,
               // so we should show the last number before the operator
               lastPart = parts[parts.length - 2];
            }
            _output = double.parse(lastPart).toString();
            if (_output.endsWith('.0')) {
              _output = _output.substring(0, _output.length - 2);
            }
          } catch (e) {
            _output = "0"; // Fallback to 0 if parsing fails
          }
        }
      }
    } else if (buttonText == "+" ||
        buttonText == "-" ||
        buttonText == "*" ||
        buttonText == "/") {
      if (_input.isNotEmpty && !_isOperator(_input.substring(_input.length - 1))) {
        num1 = double.parse(_output);
        operator = buttonText;
        _input = _input + buttonText;
        _output = "0"; // Reset output for the next number
      } else if (_input.isEmpty && buttonText == "-") {
        // Allow starting with a minus sign for negative numbers
        _input = buttonText;
        _output = buttonText; // Show '-' in output initially
        operator = ""; // No operator set yet
      }
    } else if (buttonText == ".") {
      if (_output.contains(".")) {
        print("Already contains a decimal");
        return;
      }
      if (_output == "0") {
        _output = "0.";
        _input = _input + "0.";
      } else {
        _output = _output + buttonText;
        _input = _input + buttonText;
      }
    } else if (buttonText == "=") {
      if (_input.isNotEmpty && !_isOperator(_input.substring(_input.length - 1))) {
        num2 = double.parse(_output);

        try {
          if (operator == "+") {
            _output = (num1 + num2).toString();
          } else if (operator == "-") {
            _output = (num1 - num2).toString();
          } else if (operator == "*") {
            _output = (num1 * num2).toString();
          } else if (operator == "/") {
            if (num2 == 0) {
              _output = "Error"; // Handle division by zero
            } else {
              _output = (num1 / num2).toString();
            }
          } else {
             // If no operator was set (e.g., just entered a number and pressed =),
             // keep the current output
             _output = _output;
          }

          // Format the output
          if (_output != "Error") {
            // Determine the number of decimal places to show
            int decimalPlaces = 0;
            if (_output.contains('.')) {
               // Keep up to a reasonable number of decimal places, e.g., 10
               decimalPlaces = min(10, _output.split('.').last.length);
            }
            _output = double.parse(_output).toStringAsFixed(decimalPlaces);

            // Remove trailing .0 if it exists
            if (_output.endsWith('.0')) {
              _output = _output.substring(0, _output.length - 2);
            }
             // Remove trailing zeros after decimal point
            if (_output.contains('.') && _output.endsWith('0')) {
              _output = _output.replaceAll(RegExp(r'0*$'), '');
               // If it ends with just '.', remove that too
               if (_output.endsWith('.')) {
                 _output = _output.substring(0, _output.length - 1);
               }
            }
          }

          num1 = 0.0; // Reset for next calculation
          num2 = 0.0;
          operator = "";
          _input = _output; // Show the result as the new input history

        } catch (e) {
          _output = "Error";
          num1 = 0.0;
          num2 = 0.0;
          operator = "";
          _input = "Error";
        }
      }
    } else {
      // For numbers
      if (operator.isNotEmpty && _output == "0" && buttonText != "0") {
        _output = buttonText;
        _input = _input + buttonText;
      } else if (operator.isEmpty && _output == "0" && buttonText != "0") {
        _output = buttonText;
        _input = buttonText;
      } else if (operator.isEmpty && _output != "0") {
        _output = _output + buttonText;
        _input = _input + buttonText;
      } else if (operator.isNotEmpty && _output != "0") {
        _output = _output + buttonText;
        _input = _input + buttonText;
      } else if (buttonText == "0" && _output != "0") {
        _output = _output + buttonText;
        _input = _input + buttonText;
      } else if (buttonText == "0" && _output == "0" && _input.isEmpty) {
         // Allow entering 0 initially
         _output = "0";
         _input = "0";
      } else if (buttonText == "0" && _output == "0" && _input == "0") {
         // Don't add more zeros if already showing 0
      }
       else {
         _output = _output + buttonText;
         _input = _input + buttonText;
       }
    }

    setState(() {});
  }

  Widget _buildRevisedButton(String text) {
    return Expanded(
      child: Padding(
        padding: const EdgeInsets.all(8.0), // Added padding between buttons
        child: ElevatedButton(
          onPressed: () => _buttonPressedRevised(text),
          style: ElevatedButton.styleFrom(
            foregroundColor: _isOperator(text) || text == "=" || text == "AC" || text == "DEL"
                ? Colors.cyanAccent.shade100 // Different color for operators and special buttons
                : Colors.white, // Standard color for numbers
            backgroundColor: _isOperator(text)
                ? Colors.teal.shade800 // Darker teal for operators
                : text == "="
                    ? Colors.cyan.shade800 // Distinct color for equals
                    : text == "AC" || text == "DEL"
                        ? Colors.blueGrey.shade700 // Darker background for AC/DEL
                        : Colors.blueGrey.shade800, // Standard background for numbers
            shape: RoundedRectangleBorder(
                borderRadius: BorderRadius.circular(24)), // Even more rounded buttons
            padding: const EdgeInsets.all(20),
            elevation: 8, // More prominent shadow
          ),
          child: Text(
            text,
            style: TextStyle(
                fontSize: text.length > 2 ? 20 : 28, // Smaller font for longer text
                fontWeight: FontWeight.bold),
          ),
        ),
      ),
    );
  }

  bool _isOperator(String text) {
    return text == "+" || text == "-" || text == "*" || text == "/";
  }

  @override
  Widget build(BuildContext context) {
    // Restructure the buttons slightly for better layout
    final List<List<String>> keyboardLayout = [
      ['AC', '/', '*', '-'],
      ['7', '8', '9', '+'],
      ['4', '5', '6', 'DEL'], // Added DEL
      ['1', '2', '3', '='],
      ['0', '.'], // Combine 0 and . for a wider button if needed
    ];

    return Scaffold(
      appBar: AppBar(title: const Text('Oráculo de Cálculo')), // Updated title
      body: Padding(
        padding: const EdgeInsets.all(16.0), // Adjusted padding
        child: Column(
          children: [
            Expanded(
              flex: 2, // Allocate more space for the display
              child: Container(
                alignment: Alignment.bottomRight, // Align text to bottom right
                padding: const EdgeInsets.symmetric(horizontal: 24, vertical: 12),
                decoration: BoxDecoration(
                  color: Colors.blueGrey.shade800, // Darker display background
                  borderRadius: BorderRadius.circular(16), // Rounded display corners
                  border: Border.all(color: Colors.tealAccent.shade400, width: 2), // Accent border
                  boxShadow: [ // Add subtle shadow to the display
                    BoxShadow(
                      color: Colors.black.withAlpha((255 * 0.3).round()), // Corrected withAlpha
                      spreadRadius: 1, // Corrected parameter name
                      blurRadius: 8,
                      offset: const Offset(0, 4),
                    ),
                  ],
                ),
                child: Column(
                   crossAxisAlignment: CrossAxisAlignment.end,
                   mainAxisAlignment: MainAxisAlignment.end,
                   children: [
                      Text(
                         _input,
                         style: TextStyle(fontSize: 24, color: Colors.white54), // Style for input history
                         maxLines: 1,
                         overflow: TextOverflow.ellipsis,
                      ),
                     Text(
                        _output,
                        style: const TextStyle(
                            fontSize: 48, // Larger font for result
                            fontWeight: FontWeight.bold,
                            color: Colors.white), // White text for result
                         maxLines: 1,
                         overflow: TextOverflow.ellipsis,
                     ),
                   ],
                ),
              ),
            ),
            const SizedBox(height: 20), // Spacing between display and buttons
            Expanded(
              flex: 3, // Allocate space for buttons
              child: Column(
                children: keyboardLayout.map((row) {
                  return Expanded(
                    child: Row(
                      crossAxisAlignment: CrossAxisAlignment.stretch, // Stretch buttons vertically
                      children: row.map((text) {
                        return _buildRevisedButton(text);
                      }).toList(),
                    ),
                  );
                }).toList(),
              ),
            )
          ],
        ),
      ),
    );
  }
}
