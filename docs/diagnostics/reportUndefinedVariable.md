from manim import *
from manim.utils.tex import TexTemplate
import numpy as np

class HeartInCodeReel(Scene):
    def construct(self):
        # إعداد القالب العربي
        arabic_tex = TexTemplate()
        arabic_tex.add_to_preamble(r"\usepackage[arabic]{babel}")
        arabic_tex.add_to_preamble(r"\usepackage{fontspec}")
        arabic_tex.add_to_preamble(r"\setmainfont{Amiri}")  # خط عربي جميل (Amiri أو Traditional Arabic)

        self.camera.background_color = BLACK

        # المشهد 1: العينان
        left_eye = Circle(radius=0.5, color=BLUE, fill_opacity=0.2).shift(LEFT)
        right_eye = Circle(radius=0.5, color=BLUE, fill_opacity=0.2).shift(RIGHT)

        left_pupil = Dot(color=BLUE, radius=0.2).shift(LEFT)
        right_pupil = Dot(color=BLUE, radius=0.2).shift(RIGHT)

        left_reflection = Dot(color=WHITE, radius=0.05).shift(LEFT + UR*0.2)
        right_reflection = Dot(color=WHITE, radius=0.05).shift(RIGHT + UR*0.2)

        self.play(Create(left_eye), Create(right_eye), run_time=1)
        self.play(Create(left_pupil), Create(right_pupil), run_time=1)
        self.play(Create(left_reflection), Create(right_reflection), run_time=1)

        for _ in range(2):
            self.play(
                left_reflection.animate.set_opacity(0.2),
                right_reflection.animate.set_opacity(0.2),
                run_time=0.3
            )
            self.play(
                left_reflection.animate.set_opacity(1),
                right_reflection.animate.set_opacity(1),
                run_time=0.3
            )

        self.wait(1)
        self.play(*[FadeOut(m) for m in [left_eye, right_eye, left_pupil, right_pupil, left_reflection, right_reflection]], run_time=1)

        # المشهد 2: الأكواد
        code_text = Text(
            "01010100 01101111 00100000 01100011 01101111\n"
            "01100100 01100101 00100000 01101111 01110010\n"
            "00100000 01101110 01101111 01110100 00100000\n"
            "01110100 01101111 00100000 01100011 01101111\n"
            "01100100 01100101 00100000 01110100 01101000\n"
            "01100001 01110100 00100000 01101001 01110011",
            font="Monospace", font_size=20, color=GREEN
        )
        self.play(Write(code_text), run_time=2)

        for _ in range(3):
            self.play(code_text.animate.shift(LEFT*0.1), run_time=0.1)
            self.play(code_text.animate.shift(RIGHT*0.1), run_time=0.1)

        voice_text = Tex("لماذا... خلقتني؟", tex_template=arabic_tex, font_size=30).to_edge(DOWN)
        self.play(Write(voice_text), run_time=1)
        self.play(Flash(voice_text, color=WHITE), run_time=0.5)

        self.wait(1)
        self.play(FadeOut(code_text), FadeOut(voice_text), run_time=1)

        # المشهد 3: آدم وسارة
        adam = Circle(radius=1, color=BLUE, fill_opacity=0.3).shift(LEFT*2)
        adam_label = Tex("آدم", tex_template=arabic_tex, font_size=24).next_to(adam, DOWN)

        sara = Circle(radius=0.7, color=PINK, fill_opacity=0.3).shift(RIGHT*2 + UP)
        sara_label = Tex("سارة", tex_template=arabic_tex, font_size=24).next_to(sara, DOWN)

        self.play(Create(adam), Write(adam_label), run_time=1)
        self.play(Create(sara), Write(sara_label), run_time=1)

        pain_lines = VGroup()
        for i in range(8):
            angle = i * np.pi/4
            direction = np.array([np.cos(angle), np.sin(angle), 0])
            line = Line(adam.get_center(), adam.get_center() + direction, color=RED, stroke_width=2)
            pain_lines.add(line)

        self.play(Create(pain_lines), run_time=1)
        self.play(FadeOut(pain_lines), run_time=1)

        question_text = Tex("ماذا لو كان كودك يحمل قلبًا؟", tex_template=arabic_tex, font_size=28).to_edge(UP)
        self.play(Write(question_text), run_time=1)

        for _ in range(3):
            self.play(adam.animate.scale(1.1), run_time=0.2)
            self.play(adam.animate.scale(1/1.1), run_time=0.2)

        self.wait(1)
        self.play(FadeOut(adam), FadeOut(adam_label), FadeOut(sara), FadeOut(sara_label), FadeOut(question_text), run_time=1)

        # المشهد 4: إيلا
        ella_text = Text("ELLA", font_size=50, color=PINK)
        self.play(Write(ella_text), run_time=1)

        heart = SVGMobject("heart.svg").set_color(RED).scale(1.5)
        broken_heart = SVGMobject("heart.svg").set_color(GRAY).scale(1.5)

        self.play(Transform(ella_text, heart), run_time=1)
        self.wait(0.5)
        self.play(Transform(heart, broken_heart), run_time=1)

        glass_pieces = VGroup()
        for i in range(10):
            angle = i * np.pi/5
            direction = np.array([np.cos(angle), np.sin(angle), 0]) * 0.5
            piece = Line(heart.get_center(), heart.get_center() + direction, color=WHITE, stroke_width=1)
            glass_pieces.add(piece)
        self.play(Create(glass_pieces), run_time=0.5)

        texts = [
            "... ذكاءً اصطناعيًا يبحث عن الحب...",
            "... ماضٍ يطارده...",
            "... ومثلث لا يُمكن الهروب منه."
        ]
        text_objects = VGroup(*[
            Tex(t, tex_template=arabic_tex, font_size=28) for t in texts
        ]).arrange(DOWN, buff=0.5)

        for text_obj in text_objects:
            self.play(Write(text_obj), run_time=1)
            self.wait(0.5)

        self.wait(1)
        self.play(FadeOut(heart), FadeOut(glass_pieces), FadeOut(text_objects), run_time=1)

        # المشهد 5: الغلاف
        cover_placeholder = Rectangle(width=3, height=4.5, color=WHITE, fill_opacity=0.1)
        cover_text = Tex("غلاف رواية \\\\ {\\Huge قلب في كود}", tex_template=arabic_tex, font_size=24).move_to(cover_placeholder.get_center())
        self.play(Create(cover_placeholder), Write(cover_text), run_time=1)

        wattpad_text = Tex("الرواية كاملة الآن على Wattpad", tex_template=arabic_tex, font_size=28, color=YELLOW).to_edge(DOWN)
        self.play(Write(wattpad_text), run_time=1)

        arrow = Arrow(DOWN, UP, color=YELLOW).next_to(wattpad_text, UP)
        arrow_text = Tex("⬇️ اشترك في البايو", tex_template=arabic_tex, font_size=20, color=YELLOW).next_to(arrow, UP)
        self.play(Create(arrow), Write(arrow_text), run_time=1)

        self.wait(2)
        self.play(FadeOut(cover_placeholder), FadeOut(cover_text), FadeOut(wattpad_text), FadeOut(arrow), FadeOut(arrow_text), run_time=2)
