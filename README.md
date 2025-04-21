# Food-Court-App
import 'package:flutter/material.dart';

void main() {
  runApp(FoodOrderApp());
}

class FoodOrderApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Food Court Management System',
      theme: ThemeData(primarySwatch: Colors.teal),
      home: RegisterScreen(),
    );
  }
}

class FoodItem {
  final int indexNum;
  final String disNam;
  final double price;
  final String imageUrl;

  const FoodItem({
    required this.indexNum,
    required this.disNam,
    required this.price,
    required this.imageUrl,
  });
}

class Order {
  final List<FoodItem> items;
  final DateTime timestamp;
  final double total;

  Order({required this.items, required this.timestamp, required this.total});
}

List<Order> orderHistory = [];

class RegisterScreen extends StatefulWidget {
  @override
  _RegisterScreenState createState() => _RegisterScreenState();
}

class _RegisterScreenState extends State<RegisterScreen> {
  final _formKey = GlobalKey<FormState>();
  String name = '';
  String emailOrPhone = '';
  String password = '';
  bool _obscurePassword = true;

  void registerUser() {
    if (_formKey.currentState!.validate()) {
      _formKey.currentState!.save();
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Registered Successfully as $name')),
      );
      Navigator.pushReplacement(
        context,
        MaterialPageRoute(
          builder: (_) => WelcomeScreen(name: name, emailOrPhone: emailOrPhone),
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        decoration: const BoxDecoration(
          gradient: LinearGradient(
            colors: [Colors.blueAccent, Colors.white],
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
          ),
        ),
        child: Center(
          child: SingleChildScrollView(
            padding: const EdgeInsets.all(16.0),
            child: Form(
              key: _formKey,
              child: Column(
                children: [
                  Image.network(
                    'https://cdn-icons-png.flaticon.com/512/3075/3075977.png',
                    width: 80,
                    height: 80,
                  ),
                  const SizedBox(height: 10),
                  const Text(
                    "FOOD COURT MANAGEMENT SYSTEM",
                    style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
                    textAlign: TextAlign.center,
                  ),
                  const SizedBox(height: 20),
                  TextFormField(
                    decoration: const InputDecoration(labelText: 'Full Name'),
                    onSaved: (val) => name = val!,
                    validator: (val) => val!.isEmpty ? 'Required' : null,
                  ),
                  TextFormField(
                    decoration: const InputDecoration(labelText: 'Email or Phone'),
                    onSaved: (val) => emailOrPhone = val!,
                    validator: (val) => val!.isEmpty ? 'Required' : null,
                  ),
                  TextFormField(
                    decoration: InputDecoration(
                      labelText: 'Password',
                      suffixIcon: IconButton(
                        icon: Icon(_obscurePassword ? Icons.visibility : Icons.visibility_off),
                        onPressed: () {
                          setState(() {
                            _obscurePassword = !_obscurePassword;
                          });
                        },
                      ),
                    ),
                    obscureText: _obscurePassword,
                    onSaved: (val) => password = val!,
                    validator: (val) => val!.isEmpty ? 'Required' : null,
                  ),
                  const SizedBox(height: 20),
                  ElevatedButton(
                    child: const Text("Register"),
                    onPressed: registerUser,
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

class WelcomeScreen extends StatelessWidget {
  final String name;
  final String emailOrPhone;

  WelcomeScreen({required this.name, required this.emailOrPhone});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        decoration: const BoxDecoration(
          image: DecorationImage(
            image: NetworkImage(
              'https://cdn.pixabay.com/photo/2015/04/08/13/13/food-712665_1280.jpg',
            ),
            fit: BoxFit.cover,
          ),
        ),
        child: Container(
          color: Colors.black.withOpacity(0.6),
          child: Center(
            child: Padding(
              padding: const EdgeInsets.all(24.0),
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  const Text(
                    "Welcome to the Food Court!",
                    style: TextStyle(
                      color: Colors.white,
                      fontSize: 30,
                      fontWeight: FontWeight.bold,
                      shadows: [Shadow(blurRadius: 10, color: Colors.teal)],
                    ),
                    textAlign: TextAlign.center,
                  ),
                  const SizedBox(height: 20),
                  Text(
                    "Hey $name, ready to eat like it's your last meal?",
                    style: const TextStyle(
                      color: Colors.white70,
                      fontSize: 18,
                      fontStyle: FontStyle.italic,
                    ),
                    textAlign: TextAlign.center,
                  ),
                  const SizedBox(height: 40),
                  ElevatedButton.icon(
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.teal,
                      padding: const EdgeInsets.symmetric(horizontal: 30, vertical: 15),
                    ),
                    icon: const Icon(Icons.fastfood),
                    label: const Text(
                      "Let's Order!",
                      style: TextStyle(fontSize: 18),
                    ),
                    onPressed: () {
                      Navigator.pushReplacement(
                        context,
                        MaterialPageRoute(
                          builder: (_) => MainScreen(
                              name: name, emailOrPhone: emailOrPhone),
                        ),
                      );
                    },
                  ),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }
}

// ===================== Main Screen with Bottom Navigation =====================
class MainScreen extends StatefulWidget {
  final String name;
  final String emailOrPhone;

  MainScreen({required this.name, required this.emailOrPhone});

  @override
  _MainScreenState createState() => _MainScreenState();
}

class _MainScreenState extends State<MainScreen> {
  int _selectedIndex = 0;

  final List<FoodItem> menu = [
    FoodItem(indexNum: 1, disNam: "Burger", price: 80.00, imageUrl: "https://cdn.pixabay.com/photo/2016/03/05/19/02/hamburger-1238246_1280.jpg"),
    FoodItem(indexNum: 2, disNam: "Pizza", price: 90.00, imageUrl: "https://cdn.pixabay.com/photo/2017/12/09/08/18/pizza-3007395_1280.jpg"),
    FoodItem(indexNum: 3, disNam: "Hot Coffee", price: 30.00, imageUrl: "https://cdn.pixabay.com/photo/2016/11/29/03/53/beverage-1869598_1280.jpg"),
    FoodItem(indexNum: 4, disNam: "Cold Coffee", price: 50.00, imageUrl: "https://cdn.pixabay.com/photo/2020/03/21/17/47/iced-coffee-4955197_1280.jpg"),
    FoodItem(indexNum: 5, disNam: "Tea", price: 15.00, imageUrl: "https://cdn.pixabay.com/photo/2016/03/05/20/01/tea-1237610_1280.jpg"),
    FoodItem(indexNum: 6, disNam: "Sandwich", price: 70.00, imageUrl: "https://cdn.pixabay.com/photo/2017/02/15/10/39/sandwich-2068213_1280.jpg"),
  ];

  @override
  Widget build(BuildContext context) {
    final List<Widget> _screens = [
      MenuScreen(menu: menu),
      OrderScreen(menu: menu),
      ProfileScreen(name: widget.name, emailOrPhone: widget.emailOrPhone),
      OrderHistoryScreen(),
    ];

    return Scaffold(
      appBar: AppBar(title: Text('Food Court System')),
      body: _screens[_selectedIndex],
      bottomNavigationBar: BottomNavigationBar(
        type: BottomNavigationBarType.fixed,
        currentIndex: _selectedIndex,
        selectedItemColor: Colors.teal,
        unselectedItemColor: Colors.grey,
        onTap: (index) => setState(() => _selectedIndex = index),
        items: const [
          //BottomNavigationBarItem(icon: Icon(Icons.home), label: 'Home'),
          BottomNavigationBarItem(icon: Icon(Icons.restaurant_menu), label: 'Menu'),
          BottomNavigationBarItem(icon: Icon(Icons.shopping_cart), label: 'Order'),
          BottomNavigationBarItem(icon: Icon(Icons.person), label: 'Profile'),
          BottomNavigationBarItem(icon: Icon(Icons.history), label: 'History'),
        ],
      ),
    );
  }
}

// ===================== Other Screens =====================
class MenuScreen extends StatelessWidget {
  final List<FoodItem> menu;

  MenuScreen({required this.menu});

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: menu.length,
      itemBuilder: (context, index) {
        final item = menu[index];
        return Card(
          child: ListTile(
            leading: Image.network(
              item.imageUrl,
              width: 50,
              height: 50,
              errorBuilder: (context, error, stackTrace) => Icon(Icons.image_not_supported),
            ),
            title: Text('${item.disNam} - ${item.price.toStringAsFixed(2)} tk'),
            subtitle: Text('ID: ${item.indexNum}'),
          ),
        );
      },
    );
  }
}

class OrderScreen extends StatefulWidget {
  final List<FoodItem> menu;

  OrderScreen({required this.menu});

  @override
  _OrderScreenState createState() => _OrderScreenState();
}

class _OrderScreenState extends State<OrderScreen> {
  Map<int, int> orderMap = {};

  void placeOrder() {
    double total = 0.0;
    List<FoodItem> orderedItems = [];

    widget.menu.forEach((item) {
      if (orderMap.containsKey(item.indexNum)) {
        total += item.price * orderMap[item.indexNum]!;
        orderedItems.addAll(List.filled(orderMap[item.indexNum]!, item));
      }
    });

    if (orderedItems.isNotEmpty) {
      orderHistory.insert(
        0,
        Order(items: List.from(orderedItems), timestamp: DateTime.now(), total: total),
      );
    }

    showDialog(
      context: context,
      builder: (_) => AlertDialog(
        title: Text('Order Placed!'),
        content: Text('Total Amount: ${total.toStringAsFixed(2)} tk'),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: Text('Okay'),
          )
        ],
      ),
    );

    setState(() {
      orderMap.clear();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Expanded(
          child: ListView.builder(
            itemCount: widget.menu.length,
            itemBuilder: (context, index) {
              final item = widget.menu[index];
              return ListTile(
                title: Text(item.disNam),
                subtitle: Text('${item.price} tk'),
                trailing: SizedBox(
                  width: 100,
                  child: TextField(
                    decoration: const InputDecoration(labelText: 'Qty'),
                    keyboardType: TextInputType.number,
                    onChanged: (val) {
                      int qty = int.tryParse(val) ?? 0;
                      setState(() {
                        if (qty > 0) {
                          orderMap[item.indexNum] = qty;
                        } else {
                          orderMap.remove(item.indexNum);
                        }
                      });
                    },
                  ),
                ),
              );
            },
          ),
        ),
        Padding(
          padding: const EdgeInsets.all(8.0),
          child: ElevatedButton(
            child: Text('Place Order'),
            onPressed: placeOrder,
          ),
        ),
      ],
    );
  }
}

class OrderHistoryScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: orderHistory.length,
      itemBuilder: (context, index) {
        final order = orderHistory[index];
        return Card(
          margin: EdgeInsets.all(8),
          child: ListTile(
            title: Text(
              'Order on ${order.timestamp.toLocal().toString().split(".")[0]}',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            subtitle: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                ...order.items.map((item) => Text('${item.disNam} - ${item.price} tk')),
                Text('Total: ${order.total.toStringAsFixed(2)} tk'),
              ],
            ),
          ),
        );
      },
    );
  }
}

class ProfileScreen extends StatelessWidget {
  final String name;
  final String emailOrPhone;

  ProfileScreen({required this.name, required this.emailOrPhone});

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: const EdgeInsets.all(16.0),
      decoration: const BoxDecoration(
        gradient: LinearGradient(
          colors: [Colors.white, Colors.lightBlueAccent],
          begin: Alignment.topLeft,
          end: Alignment.bottomLeft,
        ),
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Center(
            child: CircleAvatar(
              radius: 40,
              backgroundImage: NetworkImage('https://cdn-icons-png.flaticon.com/512/3135/3135715.png'),
            ),
          ),
          const SizedBox(height: 20),
          Text("Name: $name", style: TextStyle(fontSize: 18)),
          Text("Email/Phone: $emailOrPhone", style: TextStyle(fontSize: 18)),
        ],
      ),
    );
  }
}
