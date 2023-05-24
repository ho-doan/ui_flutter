# ui_flutter
<img src="./ui_1.png" witch=150/>
```dart
class RoundedCustomRectangleBorder extends OutlinedBorder {
  const RoundedCustomRectangleBorder({
    BorderSide side = BorderSide.none,
    this.borderRadius = BorderRadius.zero,
  }) : super(side: side);
  final BorderRadiusGeometry borderRadius;
  @override
  EdgeInsetsGeometry get dimensions {
    return EdgeInsets.all(side.width);
  }
  @override
  ShapeBorder scale(double t) {
    return RoundedRectangleBorder(
      side: side.scale(t),
      borderRadius: borderRadius * t,
    );
  }
  @override
  ShapeBorder? lerpFrom(ShapeBorder? a, double t) {
    if (a is RoundedRectangleBorder) {
      return RoundedRectangleBorder(
        side: BorderSide.lerp(a.side, side, t),
        borderRadius:
            BorderRadiusGeometry.lerp(a.borderRadius, borderRadius, t)!,
      );
    }
    if (a is CircleBorder) {
      return _RoundedRectangleCustomToCircleBorder(
        side: BorderSide.lerp(a.side, side, t),
        borderRadius: borderRadius,
        cirCleNess: 1.0 - t,
      );
    }
    return super.lerpFrom(a, t);
  }

  @override
  ShapeBorder? lerpTo(ShapeBorder? b, double t) {
    if (b is RoundedRectangleBorder) {
      return RoundedRectangleBorder(
        side: BorderSide.lerp(side, b.side, t),
        borderRadius:
            BorderRadiusGeometry.lerp(borderRadius, b.borderRadius, t)!,
      );
    }
    if (b is CircleBorder) {
      return _RoundedRectangleCustomToCircleBorder(
        side: BorderSide.lerp(side, b.side, t),
        borderRadius: borderRadius,
        cirCleNess: t,
      );
    }
    return super.lerpTo(b, t);
  }
  @override
  RoundedRectangleBorder copyWith(
      {BorderSide? side, BorderRadiusGeometry? borderRadius}) {
    return RoundedRectangleBorder(
      side: side ?? this.side,
      borderRadius: borderRadius ?? this.borderRadius,
    );
  }

  @override
  Path getInnerPath(Rect rect, {TextDirection? textDirection}) {
    return Path()
      ..addRRect(borderRadius
          .resolve(textDirection)
          .toRRect(rect)
          .deflate(side.width));
  }

  @override
  Path getOuterPath(Rect rect, {TextDirection? textDirection}) {
    return Path()..addRRect(borderRadius.resolve(textDirection).toRRect(rect));
  }

  @override
  void paint(Canvas canvas, Rect rect, {TextDirection? textDirection}) {
    switch (side.style) {
      case BorderStyle.none:
        break;
      case BorderStyle.solid:
        final double width = side.width;
        if (width == 0.0) {
          canvas.drawRRect(borderRadius.resolve(textDirection).toRRect(rect),
              side.toPaint());
        } else {
          final RRect outer = borderRadius.resolve(textDirection).toRRect(rect);
          final RRect inner = outer.deflate(width);
          final Paint paint = Paint()..color = side.color;
          final Paint paint2 = Paint()..color = Colors.white;

          canvas.drawDRRect(outer, inner, paint);
          canvas.drawPath(
              Path()..addPath(getTrianglePath(40, 40), const Offset(-30, -19)),
              paint);
          canvas.drawPath(
              Path()
                ..addPath(getTickPath(const Size(35, 35)), const Offset(79, 0)),
              paint2);
        }
    }
  }

  Path getTrianglePath(double x, double y) {
    return Path()
      ..moveTo(x * 2.240499, y * 0.4641165)
      ..lineTo(x * 3.039413, y * 0.4641165)
      ..arcToPoint(Offset(x * 3.240499, y * 0.6652069),
          radius: Radius.elliptical(x * 0.2010859, y * 0.2010904))
      ..lineTo(x * 3.240499, y * 1.464117)
      ..close();
  }

  Path getTickPath(Size size) {
    final Path path_0 = Path();
    path_0.moveTo(size.width * 0.1513658, size.height * 0.5394244);
    path_0.lineTo(size.width * 0.006621293, size.height * 0.3682648);
    path_0.arcToPoint(Offset(size.width * 0.006621293, size.height * 0.3311515),
        radius: Radius.elliptical(
            size.width * 0.02537649, size.height * 0.02780121));
    path_0.lineTo(size.width * 0.03812633, size.height * 0.2940383);
    path_0.arcToPoint(Offset(size.width * 0.06963136, size.height * 0.2940383),
        radius: Radius.elliptical(
            size.width * 0.02115734, size.height * 0.02317892));
    path_0.lineTo(size.width * 0.1671953, size.height * 0.4093256);
    path_0.lineTo(size.width * 0.3761818, size.height * 0.1623874);
    path_0.arcToPoint(Offset(size.width * 0.4076869, size.height * 0.1623874),
        radius: Radius.elliptical(
            size.width * 0.02115734, size.height * 0.02317892));
    path_0.lineTo(size.width * 0.4391919, size.height * 0.1995007);
    path_0.arcToPoint(Offset(size.width * 0.4391919, size.height * 0.2366139),
        radius: Radius.elliptical(
            size.width * 0.02537649, size.height * 0.02780121));
    path_0.lineTo(size.width * 0.1828709, size.height * 0.5394244);
    path_0.arcToPoint(Offset(size.width * 0.1513658, size.height * 0.5394244),
        radius: Radius.elliptical(
            size.width * 0.02115734, size.height * 0.02317892));
    path_0.close();
    return path_0;
  }

  @override
  bool operator ==(Object other) {
    if (other.runtimeType != runtimeType) {
      return false;
    }
    return other is RoundedRectangleBorder &&
        other.side == side &&
        other.borderRadius == borderRadius;
  }

  @override
  int get hashCode => Object.hash(side, borderRadius);
}

class _RoundedRectangleCustomToCircleBorder extends OutlinedBorder {
  const _RoundedRectangleCustomToCircleBorder({
    BorderSide side = BorderSide.none,
    this.borderRadius = BorderRadius.zero,
    required this.cirCleNess,
  }) : super(side: side);

  final BorderRadiusGeometry borderRadius;

  final double cirCleNess;

  @override
  EdgeInsetsGeometry get dimensions {
    return EdgeInsets.all(side.width);
  }

  @override
  ShapeBorder scale(double t) {
    return _RoundedRectangleCustomToCircleBorder(
      side: side.scale(t),
      borderRadius: borderRadius * t,
      cirCleNess: t,
    );
  }

  @override
  ShapeBorder? lerpFrom(ShapeBorder? a, double t) {
    if (a is RoundedRectangleBorder) {
      return _RoundedRectangleCustomToCircleBorder(
        side: BorderSide.lerp(a.side, side, t),
        borderRadius:
            BorderRadiusGeometry.lerp(a.borderRadius, borderRadius, t)!,
        cirCleNess: cirCleNess * t,
      );
    }
    if (a is CircleBorder) {
      return _RoundedRectangleCustomToCircleBorder(
        side: BorderSide.lerp(a.side, side, t),
        borderRadius: borderRadius,
        cirCleNess: cirCleNess + (1.0 - cirCleNess) * (1.0 - t),
      );
    }
    if (a is _RoundedRectangleCustomToCircleBorder) {
      return _RoundedRectangleCustomToCircleBorder(
        side: BorderSide.lerp(a.side, side, t),
        borderRadius:
            BorderRadiusGeometry.lerp(a.borderRadius, borderRadius, t)!,
        cirCleNess: lerpDouble(a.cirCleNess, cirCleNess, t)!,
      );
    }
    return super.lerpFrom(a, t);
  }

  @override
  ShapeBorder? lerpTo(ShapeBorder? b, double t) {
    if (b is RoundedRectangleBorder) {
      return _RoundedRectangleCustomToCircleBorder(
        side: BorderSide.lerp(side, b.side, t),
        borderRadius:
            BorderRadiusGeometry.lerp(borderRadius, b.borderRadius, t)!,
        cirCleNess: cirCleNess * (1.0 - t),
      );
    }
    if (b is CircleBorder) {
      return _RoundedRectangleCustomToCircleBorder(
        side: BorderSide.lerp(side, b.side, t),
        borderRadius: borderRadius,
        cirCleNess: cirCleNess + (1.0 - cirCleNess) * t,
      );
    }
    if (b is _RoundedRectangleCustomToCircleBorder) {
      return _RoundedRectangleCustomToCircleBorder(
        side: BorderSide.lerp(side, b.side, t),
        borderRadius:
            BorderRadiusGeometry.lerp(borderRadius, b.borderRadius, t)!,
        cirCleNess: lerpDouble(cirCleNess, b.cirCleNess, t)!,
      );
    }
    return super.lerpTo(b, t);
  }

  Rect _adjustRect(Rect rect) {
    if (cirCleNess == 0.0 || rect.width == rect.height) {
      return rect;
    }
    if (rect.width < rect.height) {
      final double delta = cirCleNess * (rect.height - rect.width) / 2.0;
      return Rect.fromLTRB(
        rect.left,
        rect.top + delta,
        rect.right,
        rect.bottom - delta,
      );
    } else {
      final double delta = cirCleNess * (rect.width - rect.height) / 2.0;
      return Rect.fromLTRB(
        rect.left + delta,
        rect.top,
        rect.right - delta,
        rect.bottom,
      );
    }
  }

  BorderRadius? _adjustBorderRadius(Rect rect, TextDirection? textDirection) {
    final BorderRadius resolvedRadius = borderRadius.resolve(textDirection);
    if (cirCleNess == 0.0) {
      return resolvedRadius;
    }
    return BorderRadius.lerp(resolvedRadius,
        BorderRadius.circular(rect.shortestSide / 2.0), cirCleNess);
  }

  @override
  Path getInnerPath(Rect rect, {TextDirection? textDirection}) {
    return Path()
      ..addRRect(_adjustBorderRadius(rect, textDirection)!
          .toRRect(_adjustRect(rect))
          .deflate(side.width));
  }

  @override
  Path getOuterPath(Rect rect, {TextDirection? textDirection}) {
    return Path()
      ..addRRect(
          _adjustBorderRadius(rect, textDirection)!.toRRect(_adjustRect(rect)));
  }

  @override
  _RoundedRectangleCustomToCircleBorder copyWith(
      {BorderSide? side,
      BorderRadiusGeometry? borderRadius,
      double? cirCleNess}) {
    return _RoundedRectangleCustomToCircleBorder(
      side: side ?? this.side,
      borderRadius: borderRadius ?? this.borderRadius,
      cirCleNess: cirCleNess ?? this.cirCleNess,
    );
  }

  @override
  void paint(Canvas canvas, Rect rect, {TextDirection? textDirection}) {
    switch (side.style) {
      case BorderStyle.none:
        break;
      case BorderStyle.solid:
        final double width = side.width;
        if (width == 0.0) {
          canvas.drawRRect(
              _adjustBorderRadius(rect, textDirection)!
                  .toRRect(_adjustRect(rect)),
              side.toPaint());
        } else {
          final RRect outer = _adjustBorderRadius(rect, textDirection)!
              .toRRect(_adjustRect(rect));
          final RRect inner = outer.deflate(width);
          final Paint paint = Paint()..color = side.color;
          canvas.drawDRRect(outer, inner, paint);
        }
    }
  }

  @override
  bool operator ==(Object other) {
    if (other.runtimeType != runtimeType) {
      return false;
    }
    return other is _RoundedRectangleCustomToCircleBorder &&
        other.side == side &&
        other.borderRadius == borderRadius &&
        other.cirCleNess == cirCleNess;
  }

  @override
  int get hashCode => Object.hash(side, borderRadius, cirCleNess);

  @override
  String toString() {
    return 'RoundedRectangleBorder($side, $borderRadius, ${(cirCleNess * 100).toStringAsFixed(1)}% of the way to being a CircleBorder)';
  }
}
```
# USE
```dart
Card(
  shape: RoundedCustomRectangleBorder(
      borderRadius: BorderRadius.circular(10),
      side: const BorderSide(color: Colors.red)),
  child: const SizedBox(
    width: 100,
    height: 100,
  ),
)
```
