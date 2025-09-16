// main.dart

import 'package:flutter/material.dart';
import 'dart:math';

// This enum will hold the different emoji types we can draw.
enum EmojiType {
  partyFace,
  heart,
  smiley, // Task 2: Smiley face
}

void main() {
  runApp(const ShapesDemoApp());
}

class ShapesDemoApp extends StatelessWidget {
  const ShapesDemoApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Emoji Drawing App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        useMaterial3: true,
      ),
      home: const ShapesDrawingScreen(),
    );
  }
}

class ShapesDrawingScreen extends StatefulWidget {
  const ShapesDrawingScreen({super.key});

  @override
  State<ShapesDrawingScreen> createState() => _ShapesDrawingScreenState();
}

class _ShapesDrawingScreenState extends State<ShapesDrawingScreen> {
  // State variable to hold the currently selected emoji.
  EmojiType _selectedEmoji = EmojiType.partyFace;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Interactive Emoji Drawing'),
        centerTitle: true,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            // Task 1: Replaces the existing CustomPaint with a new one that adheres to the "Draw shapes instead of lines" rule.
            const Text(
              'Task 1: Basic Shapes',
              style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 10),
            SizedBox(
              height: 200,
              child: CustomPaint(
                painter: BasicShapesPainter(),
                size: const Size(double.infinity, 200),
              ),
            ),
            const SizedBox(height: 20),
            const Text(
              'Interactive Emoji Drawing',
              style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 10),
            // Emoji selection dropdown menu
            Container(
              alignment: Alignment.center,
              padding: const EdgeInsets.symmetric(horizontal: 16),
              margin: const EdgeInsets.symmetric(horizontal: 50, vertical: 10),
              decoration: BoxDecoration(
                border: Border.all(color: Colors.blueGrey, width: 2),
                borderRadius: BorderRadius.circular(12),
              ),
              child: DropdownButtonHideUnderline(
                child: DropdownButton<EmojiType>(
                  isExpanded: true,
                  value: _selectedEmoji,
                  icon: const Icon(Icons.arrow_drop_down),
                  items: const [
                    DropdownMenuItem(
                      value: EmojiType.partyFace,
                      child: Text('Party Face 🎉'),
                    ),
                    DropdownMenuItem(
                      value: EmojiType.heart,
                      child: Text('Heart ❤️'),
                    ),
                    DropdownMenuItem(
                      value: EmojiType.smiley,
                      child: Text('Smiley 😊'),
                    ),
                  ],
                  onChanged: (EmojiType? newValue) {
                    setState(() {
                      _selectedEmoji = newValue!;
                    });
                  },
                ),
              ),
            ),
            const SizedBox(height: 20),
            // The canvas where the selected emoji will be drawn.
            SizedBox(
              height: 300,
              child: CustomPaint(
                painter: InteractiveEmojiPainter(emojiType: _selectedEmoji),
                size: const Size(double.infinity, 300),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

/// Task 1: This painter replaces the original one to remove lines and only draw shapes.
class BasicShapesPainter extends CustomPainter {
  @override
  void paint(Canvas canvas, Size size) {
    final centerX = size.width / 2;
    final centerY = size.height / 2;

    // Draw a square
    final squarePaint = Paint()
      ..color = Colors.blue
      ..style = PaintingStyle.fill;
    canvas.drawRect(
      Rect.fromCenter(center: Offset(centerX - 80, centerY), width: 60, height: 60),
      squarePaint,
    );

    // Draw a circle
    final circlePaint = Paint()
      ..color = Colors.red
      ..style = PaintingStyle.fill;
    canvas.drawCircle(Offset(centerX, centerY), 30, circlePaint);

    // Draw an arc
    final arcPaint = Paint()
      ..color = Colors.green
      ..style = PaintingStyle.stroke
      ..strokeWidth = 5;
    canvas.drawArc(
      Rect.fromCenter(center: Offset(centerX + 80, centerY), width: 60, height: 60),
      0, // start angle in radians
      2.1, // sweep angle in radians (about 120 degrees)
      false, // whether to use center
      arcPaint,
    );

    // Draw a rectangle
    final rectPaint = Paint()
      ..color = Colors.orange
      ..style = PaintingStyle.fill;
    canvas.drawRect(
      Rect.fromCenter(center: Offset(centerX - 160, centerY), width: 80, height: 40),
      rectPaint,
    );
    
    // Draw an oval
    final ovalPaint = Paint()
      ..color = Colors.teal
      ..style = PaintingStyle.fill;
    canvas.drawOval(
      Rect.fromCenter(center: Offset(centerX + 160, centerY), width: 80, height: 40),
      ovalPaint,
    );

    // Adding a new circle to replace the old line.
    final newCirclePaint = Paint()
      ..color = Colors.purple
      ..style = PaintingStyle.fill;
    canvas.drawCircle(Offset(centerX, centerY - 80), 20, newCirclePaint);
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) {
    return false;
  }
}

/// Task 2, Part 1 & 3: This painter handles the drawing of different emojis.
class InteractiveEmojiPainter extends CustomPainter {
  final EmojiType emojiType;

  InteractiveEmojiPainter({required this.emojiType});

  @override
  void paint(Canvas canvas, Size size) {
    final centerX = size.width / 2;
    final centerY = size.height / 2;

    switch (emojiType) {
      case EmojiType.partyFace:
        _drawPartyFace(canvas, Offset(centerX, centerY));
        break;
      case EmojiType.heart:
        _drawHeart(canvas, Offset(centerX, centerY));
        break;
      case EmojiType.smiley:
        _drawSmiley(canvas, Offset(centerX, centerY));
        break;
    }
  }

  // Helper method for Task 2: Drawing an Emoji
  void _drawSmiley(Canvas canvas, Offset center) {
    // Draw the face
    final facePaint = Paint()
      ..color = Colors.yellow
      ..style = PaintingStyle.fill;
    canvas.drawCircle(center, 100, facePaint);

    // Draw the left eye
    final eyePaint = Paint()
      ..color = Colors.black
      ..style = PaintingStyle.fill;
    canvas.drawCircle(Offset(center.dx - 40, center.dy - 30), 10, eyePaint);

    // Draw the right eye
    canvas.drawCircle(Offset(center.dx + 40, center.dy - 30), 10, eyePaint);

    // Draw the smile (an arc)
    final smilePaint = Paint()
      ..color = Colors.black
      ..style = PaintingStyle.stroke
      ..strokeWidth = 8;
    final smileRect = Rect.fromCircle(center: center, radius: 60);
    canvas.drawArc(smileRect, 0.4, 2.5, false, smilePaint);
  }

  // Task 2, Part 1 & 3: Draw a Party Face emoji with confetti.
  void _drawPartyFace(Canvas canvas, Offset center) {
    // Draw the face
    final facePaint = Paint()..color = Colors.yellow;
    canvas.drawCircle(center, 100, facePaint);

    // Left eye
    final eyePaint = Paint()..color = Colors.black;
    canvas.drawCircle(Offset(center.dx - 40, center.dy - 30), 10, eyePaint);

    // Right eye
    canvas.drawCircle(Offset(center.dx + 40, center.dy - 30), 10, eyePaint);

    // Smile (an arc)
    final smilePaint = Paint()
      ..color = Colors.black
      ..style = PaintingStyle.stroke
      ..strokeWidth = 8;
    final smileRect = Rect.fromCircle(center: center, radius: 60);
    canvas.drawArc(smileRect, 0.4, 2.5, false, smilePaint);

    // Party hat
    final hatPaint = Paint()
      ..color = Colors.red
      ..style = PaintingStyle.fill;
    final hatPath = Path()
      ..moveTo(center.dx + 60, center.dy - 100)
      ..lineTo(center.dx + 110, center.dy - 200)
      ..lineTo(center.dx + 160, center.dy - 100)
      ..close();
    canvas.drawPath(hatPath, hatPaint);

    // Confetti (Task 2, Part 2: Added detail)
    final random = Random();
    for (int i = 0; i < 50; i++) {
      final confettiPaint = Paint()..color = Color.fromRGBO(
          random.nextInt(256),
          random.nextInt(256),
          random.nextInt(256),
          1.0
      );
      final confettiOffset = Offset(
        center.dx - 150 + random.nextDouble() * 300,
        center.dy - 150 + random.nextDouble() * 300,
      );
      canvas.drawCircle(confettiOffset, random.nextDouble() * 5, confettiPaint);
    }
  }

  // Task 2, Part 1 & 3: Draw a Heart emoji with a gradient.
  void _drawHeart(Canvas canvas, Offset center) {
    final heartPath = Path();
    heartPath.moveTo(center.dx, center.dy + 80);
    heartPath.cubicTo(
      center.dx + 120, center.dy - 20,
      center.dx + 40, center.dy - 120,
      center.dx, center.dy - 60,
    );
    heartPath.cubicTo(
      center.dx - 40, center.dy - 120,
      center.dx - 120, center.dy - 20,
      center.dx, center.dy + 80,
    );

    // Part 2 & 3: Fill the heart with a gradient.
    final rect = Rect.fromCircle(center: center, radius: 150);
    final heartGradient = LinearGradient(
      begin: Alignment.topCenter,
      end: Alignment.bottomCenter,
      colors: [
        Colors.pink.shade200,
        Colors.red.shade900,
      ],
    );

    final heartPaint = Paint()
      ..shader = heartGradient.createShader(rect)
      ..style = PaintingStyle.fill;

    canvas.drawPath(heartPath, heartPaint);
  }

  @override
  bool shouldRepaint(covariant InteractiveEmojiPainter oldDelegate) {
    return oldDelegate.emojiType != emojiType;
  }
}