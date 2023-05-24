# ui_flutter

# UI 1

<img src="./assets/ui_1.png" witch=150/>

## USE

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

[Go to code](#ui-1)

# UI 2

<img src="./assets/ui_2.png" witch=150/>

## USE

```dart
CupelationSwitchCustom(
  onChanged: (value) {
    setState(() => everyDays = !everyDays);
  },
  value: everyDays,
  activeColor: AppColors.primary900,
)
```

[Go to code](#ui-2)

`#ui-1`

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

`#ui-2`

```dart
class CupelationSwitchCustom extends StatefulWidget {
  const CupelationSwitchCustom({
    super.key,
    required this.value,
    required this.onChanged,
    this.activeColor,
    this.trackColor,
    this.thumbColor,
    this.dragStartBehavior = DragStartBehavior.start,
    this.width = 50,
  });
  final bool value;
  final ValueChanged<bool>? onChanged;
  final Color? activeColor;
  final Color? trackColor;
  final Color? thumbColor;
  final DragStartBehavior dragStartBehavior;
  final double width;

  @override
  State<CupelationSwitchCustom> createState() => _CupelationSwitchCustomState();
}

class _CupelationSwitchCustomState extends State<CupelationSwitchCustom>
    with TickerProviderStateMixin {
  late TapGestureRecognizer _tap;
  late HorizontalDragGestureRecognizer _drag;

  late AnimationController _positionController;
  late CurvedAnimation position;

  late AnimationController _reactionController;
  late Animation<double> _reaction;

  bool get isInteractive => widget.onChanged != null;
  bool needsPositionAnimation = false;

  late double _kTrackWidth;
  late double _kTrackHeight;
  late double _kTrackRadius;
  late double _kTrackInnerStart;
  late double _kTrackInnerEnd;
  late double _kTrackInnerLength;
  late double radius;

  @override
  void initState() {
    super.initState();

    _kTrackWidth = widget.width;
    _kTrackHeight = widget.width / 1.9;
    _kTrackRadius = _kTrackHeight / 2.0;
    _kTrackInnerStart = _kTrackHeight / 2.0;
    _kTrackInnerEnd = _kTrackWidth - _kTrackInnerStart - 2;
    _kTrackInnerLength = _kTrackInnerEnd - _kTrackInnerStart;
    radius = _kTrackHeight / 2;

    _tap = TapGestureRecognizer()
      ..onTapDown = _handleTapDown
      ..onTapUp = _handleTapUp
      ..onTap = _handleTap
      ..onTapCancel = _handleTapCancel;
    _drag = HorizontalDragGestureRecognizer()
      ..onStart = _handleDragStart
      ..onUpdate = _handleDragUpdate
      ..onEnd = _handleDragEnd
      ..dragStartBehavior = widget.dragStartBehavior;

    _positionController = AnimationController(
      duration: _kToggleDuration,
      value: widget.value ? 1.0 : 0.0,
      vsync: this,
    );
    position = CurvedAnimation(
      parent: _positionController,
      curve: Curves.linear,
    );
    _reactionController = AnimationController(
      duration: _kReactionDuration,
      vsync: this,
    );
    _reaction = CurvedAnimation(
      parent: _reactionController,
      curve: Curves.ease,
    );
  }

  @override
  void didUpdateWidget(CupelationSwitchCustom oldWidget) {
    super.didUpdateWidget(oldWidget);
    _drag.dragStartBehavior = widget.dragStartBehavior;

    if (needsPositionAnimation || oldWidget.value != widget.value) {
      _resumePositionAnimation(isLinear: needsPositionAnimation);
    }
  }

  void _resumePositionAnimation({bool isLinear = true}) {
    needsPositionAnimation = false;
    position
      ..curve = isLinear ? Curves.linear : Curves.ease
      ..reverseCurve = isLinear ? Curves.linear : Curves.ease.flipped;
    if (widget.value) {
      _positionController.forward();
    } else {
      _positionController.reverse();
    }
  }

  void _handleTapDown(TapDownDetails details) {
    if (isInteractive) {
      needsPositionAnimation = false;
    }
    _reactionController.forward();
  }

  void _handleTap() {
    if (isInteractive) {
      widget.onChanged!(!widget.value);
      _emitVibration();
    }
  }

  void _handleTapUp(TapUpDetails details) {
    if (isInteractive) {
      needsPositionAnimation = false;
      _reactionController.reverse();
    }
  }

  void _handleTapCancel() {
    if (isInteractive) {
      _reactionController.reverse();
    }
  }

  void _handleDragStart(DragStartDetails details) {
    if (isInteractive) {
      needsPositionAnimation = false;
      _reactionController.forward();
      _emitVibration();
    }
  }

  void _handleDragUpdate(DragUpdateDetails details) {
    if (isInteractive) {
      position
        ..curve = Curves.linear
        ..reverseCurve = Curves.linear;
      final double delta = details.primaryDelta! / _kTrackInnerLength;
      switch (Directionality.of(context)) {
        case TextDirection.rtl:
          _positionController.value -= delta;
          break;
        case TextDirection.ltr:
          _positionController.value += delta;
          break;
      }
    }
  }

  void _handleDragEnd(DragEndDetails details) {
    // Deferring the animation to the next build phase.
    setState(() {
      needsPositionAnimation = true;
    });
    // Call onChanged when the user's intent to change value is clear.
    if (position.value >= 0.5 != widget.value) {
      widget.onChanged!(!widget.value);
    }
    _reactionController.reverse();
  }

  void _emitVibration() {
    switch (defaultTargetPlatform) {
      case TargetPlatform.iOS:
        HapticFeedback.lightImpact();
        break;
      case TargetPlatform.android:
      case TargetPlatform.fuchsia:
      case TargetPlatform.linux:
      case TargetPlatform.macOS:
      case TargetPlatform.windows:
        break;
    }
  }

  @override
  Widget build(BuildContext context) {
    if (needsPositionAnimation) {
      _resumePositionAnimation();
    }
    return MouseRegion(
      cursor: isInteractive && kIsWeb
          ? SystemMouseCursors.click
          : MouseCursor.defer,
      child: Opacity(
        opacity:
            widget.onChanged == null ? _kCupertinoSwitchDisabledOpacity : 1.0,
        child: _CupertinoSwitchRenderObjectWidget(
          value: widget.value,
          activeColor: CupertinoDynamicColor.resolve(
            widget.activeColor ?? CupertinoColors.systemGreen,
            context,
          ),
          trackColor: CupertinoDynamicColor.resolve(
              widget.trackColor ?? CupertinoColors.secondarySystemFill,
              context),
          thumbColor: CupertinoDynamicColor.resolve(
              widget.thumbColor ?? CupertinoColors.white, context),
          onChanged: widget.onChanged,
          textDirection: Directionality.of(context),
          state: this,
          additionalConstraints: BoxConstraints.tightFor(
            width: _kTrackWidth,
            height: _kTrackHeight,
          ),
          kTrackWidth: _kTrackWidth,
          radius: radius,
          kTrackHeight: _kTrackHeight,
          kTrackInnerEnd: _kTrackInnerEnd,
          kTrackInnerStart: _kTrackInnerStart,
          kTrackRadius: _kTrackRadius,
        ),
      ),
    );
  }

  @override
  void dispose() {
    _tap.dispose();
    _drag.dispose();

    _positionController.dispose();
    _reactionController.dispose();
    super.dispose();
  }
}

class _CupertinoSwitchRenderObjectWidget extends LeafRenderObjectWidget {
  const _CupertinoSwitchRenderObjectWidget({
    required this.value,
    required this.activeColor,
    required this.trackColor,
    required this.thumbColor,
    required this.onChanged,
    required this.textDirection,
    required this.state,
    required this.additionalConstraints,
    required this.kTrackWidth,
    required this.radius,
    required this.kTrackHeight,
    required this.kTrackInnerEnd,
    required this.kTrackInnerStart,
    required this.kTrackRadius,
  });

  final bool value;
  final Color activeColor;
  final Color trackColor;
  final Color thumbColor;
  final ValueChanged<bool>? onChanged;
  final _CupelationSwitchCustomState state;
  final TextDirection textDirection;
  final BoxConstraints additionalConstraints;
  final double kTrackWidth;
  final double radius;
  final double kTrackHeight;
  final double kTrackInnerEnd;
  final double kTrackInnerStart;
  final double kTrackRadius;

  @override
  _RenderCupertinoSwitch createRenderObject(BuildContext context) {
    return _RenderCupertinoSwitch(
      value: value,
      activeColor: activeColor,
      trackColor: trackColor,
      thumbColor: thumbColor,
      onChanged: onChanged,
      textDirection: textDirection,
      state: state,
      additionalConstraints: additionalConstraints,
      kTrackWidth: kTrackWidth,
      radius: radius,
      kTrackHeight: kTrackHeight,
      kTrackInnerEnd: kTrackInnerEnd,
      kTrackInnerStart: kTrackInnerStart,
      kTrackRadius: kTrackRadius,
    );
  }

  @override
  void updateRenderObject(
      BuildContext context, _RenderCupertinoSwitch renderObject) {
    renderObject
      ..value = value
      ..activeColor = activeColor
      ..trackColor = trackColor
      ..thumbColor = thumbColor
      ..onChanged = onChanged
      ..textDirection = textDirection;
  }
}

// Opacity of a disabled switch, as eye-balled from iOS Simulator on Mac.
const double _kCupertinoSwitchDisabledOpacity = 0.5;

const Duration _kReactionDuration = Duration(milliseconds: 300);
const Duration _kToggleDuration = Duration(milliseconds: 200);

class _RenderCupertinoSwitch extends RenderConstrainedBox {
  _RenderCupertinoSwitch({
    required bool value,
    required Color activeColor,
    required Color trackColor,
    required Color thumbColor,
    ValueChanged<bool>? onChanged,
    required TextDirection textDirection,
    required _CupelationSwitchCustomState state,
    required super.additionalConstraints,
    required this.kTrackWidth,
    required this.radius,
    required this.kTrackHeight,
    required this.kTrackInnerStart,
    required this.kTrackInnerEnd,
    required this.kTrackRadius,
  })  : _value = value,
        _activeColor = activeColor,
        _trackColor = trackColor,
        _thumbPainter = CupertinoThumbPainter.switchThumb(color: thumbColor),
        _onChanged = onChanged,
        _textDirection = textDirection,
        _state = state {
    state.position.addListener(markNeedsPaint);
    state._reaction.addListener(markNeedsPaint);
  }

  final double kTrackWidth;
  final double radius;
  final double kTrackHeight;
  final double kTrackInnerStart;
  final double kTrackRadius;
  final double kTrackInnerEnd;

  final _CupelationSwitchCustomState _state;

  bool _value;
  set value(bool value) {
    if (value == _value) {
      return;
    }
    _value = value;
    markNeedsSemanticsUpdate();
  }

  Color _activeColor;
  set activeColor(Color value) {
    if (value == _activeColor) {
      return;
    }
    _activeColor = value;
    markNeedsPaint();
  }

  Color _trackColor;
  set trackColor(Color value) {
    if (value == _trackColor) {
      return;
    }
    _trackColor = value;
    markNeedsPaint();
  }

  CupertinoThumbPainter _thumbPainter;
  set thumbColor(Color value) {
    if (value == _thumbPainter.color) {
      return;
    }
    _thumbPainter = CupertinoThumbPainter.switchThumb(color: value);
    markNeedsPaint();
  }

  ValueChanged<bool>? _onChanged;
  set onChanged(ValueChanged<bool>? value) {
    if (value == _onChanged) {
      return;
    }
    final bool wasInteractive = isInteractive;
    _onChanged = value;
    if (wasInteractive != isInteractive) {
      markNeedsPaint();
      markNeedsSemanticsUpdate();
    }
  }

  TextDirection _textDirection;
  set textDirection(TextDirection value) {
    if (_textDirection == value) {
      return;
    }
    _textDirection = value;
    markNeedsPaint();
  }

  bool get isInteractive => _onChanged != null;

  @override
  bool hitTestSelf(Offset position) => true;

  @override
  void handleEvent(PointerEvent event, BoxHitTestEntry entry) {
    assert(debugHandleEvent(event, entry));
    if (event is PointerDownEvent && isInteractive) {
      _state._drag.addPointer(event);
      _state._tap.addPointer(event);
    }
  }

  @override
  void describeSemanticsConfiguration(SemanticsConfiguration config) {
    super.describeSemanticsConfiguration(config);

    if (isInteractive) {
      config.onTap = _state._handleTap;
    }

    config.isEnabled = isInteractive;
    config.isToggled = _value;
  }

  @override
  void paint(PaintingContext context, Offset offset) {
    final Canvas canvas = context.canvas;

    final double currentValue = _state.position.value;
    final double currentReactionValue = _state._reaction.value;

    final double visualPosition;
    switch (_textDirection) {
      case TextDirection.rtl:
        visualPosition = 1.0 - currentValue;
        break;
      case TextDirection.ltr:
        visualPosition = currentValue;
        break;
    }

    final Paint paint = Paint()
      ..color = Color.lerp(_trackColor, _activeColor, currentValue)!;

    final Rect trackRect = Rect.fromLTWH(
      offset.dx + (size.width - kTrackWidth) / 2.0,
      offset.dy + (size.height - kTrackHeight) / 2.0,
      kTrackWidth,
      kTrackHeight,
    );
    final RRect trackRRect =
        RRect.fromRectAndRadius(trackRect, Radius.circular(kTrackRadius));
    canvas.drawRRect(trackRRect, paint);

    final double currentThumbExtension =
        CupertinoThumbPainter.extension * currentReactionValue;
    final double thumbLeft = lerpDouble(
      trackRect.left + kTrackInnerStart - radius,
      trackRect.left + kTrackInnerEnd - radius - currentThumbExtension,
      visualPosition,
    )!;
    final double thumbRight = lerpDouble(
      trackRect.left + kTrackInnerStart + radius + currentThumbExtension,
      trackRect.left + kTrackInnerEnd + radius,
      visualPosition,
    )!;
    final double thumbCenterY = offset.dy + size.height / 2.0;
    final Rect thumbBounds = Rect.fromLTRB(
      thumbLeft + 2,
      thumbCenterY - radius + 2,
      thumbRight - 2,
      thumbCenterY + radius - 2,
    );

    _clipRRectLayer.layer = context
        .pushClipRRect(needsCompositing, Offset.zero, thumbBounds, trackRRect,
            (PaintingContext innerContext, Offset offset) {
      _thumbPainter.paint(innerContext.canvas, thumbBounds);
    }, oldLayer: _clipRRectLayer.layer);
  }

  final LayerHandle<ClipRRectLayer> _clipRRectLayer =
      LayerHandle<ClipRRectLayer>();

  @override
  void dispose() {
    _clipRRectLayer.layer = null;
    super.dispose();
  }
}
```
