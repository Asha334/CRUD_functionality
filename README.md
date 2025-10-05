
import 'package:flutter/material.dart';
void main() => runApp(MaterialApp(home: Crud(), debugShowCheckedModeBanner: false));
class Crud extends StatefulWidget {
  @override
  CrudState createState() => CrudState();
}
class CrudState extends State<Crud> {
  List<String> items = [];
  final c = TextEditingController();
  void save([int? i]) {
    if (c.text.isEmpty) return;
    setState(() {
      i == null ? items.add(c.text) : items[i] = c.text;
      c.clear();
    });
    if (i != null) Navigator.pop(context);
  }
  void edit(int i) {
    c.text = items[i];
    showDialog(
      context: context,
      builder: (_) => AlertDialog(
        content: TextField(controller: c),
        actions: [TextButton(onPressed: () => save(i), child: Text('OK'))],
      ),
    );
  }
  @override
  Widget build(BuildContext ctx) => Scaffold(
    appBar: AppBar(title: Text('CRUD')),
    body: Column(children: [
      Row(children: [
        Expanded(child: TextField(controller: c)),
        IconButton(onPressed: () => save(), icon: Icon(Icons.add))
      ]),
      Expanded(
        child: ListView.builder(
          itemCount: items.length,
          itemBuilder: (_, i) => ListTile(
            title: Text(items[i]),
            trailing: Row(mainAxisSize: MainAxisSize.min, children: [
              IconButton(onPressed: () => edit(i), icon: Icon(Icons.edit)),
              IconButton(onPressed: () => setState(() => items.removeAt(i)), icon: Icon(Icons.delete)),
            ]),
          ),
        ),
      )
    ]),
  );
}

