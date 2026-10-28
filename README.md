# adept-learn-
this website is for learning and test your ablility 
import os
import json
import random
from datetime import datetime

import streamlit as st

st.set_page_config(
    page_title="AdeptLearn | AI Learning Studio",
    page_icon="🚀",
    layout="wide",
    initial_sidebar_state="expanded",
)

# ------------------------------ Theme ---------------------------------------
st.markdown("""
<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&family=Space+Grotesk:wght@400;500;600;700&display=swap');
:root { --bg:#080B18; --panel:#11172B; --panel2:#171D36; --muted:#A8B2D1; --text:#F4F6FF; --purple:#8B5CF6; --blue:#4F8CFF; --cyan:#38D9D6; --pink:#F472B6; }
html, body, [class*="css"] { font-family:'DM Sans',sans-serif; }
.stApp { background: radial-gradient(ellipse at 8% 0%, rgba(83,69,194,.22), transparent 34%), radial-gradient(ellipse at 90% 10%, rgba(28,160,204,.12), transparent 28%), var(--bg); color:var(--text); }
header[data-testid="stHeader"] { background:transparent; }
.block-container { padding-top:1.5rem; padding-bottom:2.5rem; max-width:1500px; }
h1,h2,h3,h4 { font-family:'Space Grotesk',sans-serif !important; color:#F7F8FF; letter-spacing:-.03em; }
p, label, .stMarkdown { color:#DCE2F8; }
[data-testid="stSidebar"] { background:linear-gradient(180deg,#10152A 0%,#0B1020 100%); border-right:1px solid rgba(157,170,255,.12); }
[data-testid="stSidebar"] .block-container { padding-top:1.4rem; }
.brand { font-family:'Space Grotesk',sans-serif; font-weight:700; font-size:1.65rem; letter-spacing:-.06em; margin-bottom:.25rem; }
.brand span { background:linear-gradient(90deg,#9B7BFF,#45D7E8); -webkit-background-clip:text; color:transparent; }
.eyebrow { color:#9CA9D2; text-transform:uppercase; letter-spacing:.16em; font-size:.72rem; font-weight:700; }
.hero { position:relative; overflow:hidden; border:1px solid rgba(165,153,255,.28); border-radius:24px; padding:30px 32px; background:linear-gradient(115deg,rgba(98,70,222,.38),rgba(25,101,166,.25) 55%,rgba(17,24,48,.8)); box-shadow:0 18px 60px rgba(20,14,65,.25); }
.hero:after { content:""; position:absolute; width:230px;height:230px;right:-60px;top:-100px;border-radius:50%;background:rgba(99,102,241,.2);filter:blur(5px); }
.hero h1 { font-size:clamp(1.8rem,3vw,2.8rem); margin:.45rem 0 .55rem; }
.hero p { color:#D7DDF7; max-width:650px; }
.panel { background:linear-gradient(145deg,rgba(22,29,54,.94),rgba(14,19,37,.94)); border:1px solid rgba(146,161,225,.14); border-radius:18px; padding:20px; box-shadow:0 10px 35px rgba(0,0,0,.12); }
.metric { background:rgba(255,255,255,.025); border:1px solid rgba(146,161,225,.13); border-radius:15px; padding:15px 16px; min-height:108px; }
.metric-label { color:#AAB5D6; font-size:.82rem; }
.metric-value { font-family:'Space Grotesk',sans-serif; font-size:1.8rem; font-weight:700; margin-top:4px; color:#F7F8FF; }
.metric-foot { color:#51D9C6; font-size:.75rem; margin-top:2px; }
.feature { border-radius:17px; padding:18px; min-height:155px; border:1px solid rgba(160,171,245,.16); background:linear-gradient(140deg,rgba(91,71,195,.22),rgba(20,29,53,.92)); }
.feature.teal { background:linear-gradient(140deg,rgba(18,164,157,.2),rgba(20,29,53,.92)); }
.feature.pink { background:linear-gradient(140deg,rgba(214,75,155,.17),rgba(20,29,53,.92)); }
.feature.blue { background:linear-gradient(140deg,rgba(55,125,230,.22),rgba(20,29,53,.92)); }
.feature-icon { font-size:1.5rem; }
.muted { color:#AAB5D6; font-size:.88rem; }
.tag { display:inline-block; border-radius:100px; padding:4px 10px; background:rgba(124,99,255,.16); border:1px solid rgba(150,130,255,.25); color:#C6BAFF; font-size:.72rem; margin-right:5px; }
.stButton > button, .stDownloadButton > button { border:0; border-radius:12px; padding:.58rem 1.05rem; color:white; font-weight:700; background:linear-gradient(100deg,#7857F5,#398EF7); box-shadow:0 6px 22px rgba(87,91,245,.22); transition:transform .15s ease,filter .15s ease; }
.stButton > button:hover, .stDownloadButton > button:hover { filter:brightness(1.12); transform:translateY(-1px); color:white; }
.stTextInput input, .stTextArea textarea, .stSelectbox [data-baseweb="select"], .stNumberInput input { background:#10172B !important; color:#F4F6FF !important; border-color:#303A5B !important; border-radius:11px !important; }
.stProgress > div > div { background:linear-gradient(90deg,#8B5CF6,#3AC8E7); }
div[data-testid="stTabs"] button { color:#AAB5D6; }
div[data-testid="stTabs"] button[aria-selected="true"] { color:#FFFFFF; border-bottom-color:#8B5CF6; }
hr { border-color:rgba(157,170,255,.13); }
small { color:#AAB5D6; }
@media (max-width: 760px) {
 .block-container { padding:1rem .8rem 2rem; }
 .hero { padding:22px 20px; border-radius:18px; }
 .panel { padding:15px; }
 .metric-value { font-size:1.45rem; }
}
</style>
""", unsafe_allow_html=True)

# ------------------------------ State ---------------------------------------
if "page" not in st.session_state: st.session_state.page = "Overview"
if "topic" not in st.session_state: st.session_state.topic = "Python functions"
if "level" not in st.session_state: st.session_state.level = "Beginner"
if "study_hours" not in st.session_state: st.session_state.study_hours = 4
if "quiz_history" not in st.session_state: st.session_state.quiz_history = []
if "completed" not in st.session_state: st.session_state.completed = set()
if "generated_content" not in st.session_state: st.session_state.generated_content = ""
if "quiz" not in st.session_state: st.session_state.quiz = []
if "quiz_index" not in st.session_state: st.session_state.quiz_index = 0
if "quiz_score" not in st.session_state: st.session_state.quiz_score = 0
if "quiz_answers" not in st.session_state: st.session_state.quiz_answers = {}
if "quiz_finished" not in st.session_state: st.session_state.quiz_finished = False
if "streak" not in st.session_state: st.session_state.streak = 6

def current_level():
    history = st.session_state.quiz_history
    if not history: return st.session_state.level
    recent = history[-3:]
    avg = sum(x["percent"] for x in recent) / len(recent)
    if avg >= 85: return "Advanced"
    if avg >= 60: return "Intermediate"
    return "Beginner"

def make_content(topic, level, kind, goals):
    key = os.getenv("OPENAI_API_KEY", "").strip()
    if key:
        try:
            from openai import OpenAI
            client = OpenAI(api_key=key)
            prompt = f"""You are AdeptLearn, a supportive personalized tutor. Create {kind} about: {topic}.
Learner level: {level}. Learning goal: {goals or 'Understand the topic and practice it'}.
Use simple, accurate language suitable for the level. Structure with clear Markdown headings.
For notes include a short explanation, key points, a worked example and a 3-question self-check.
For practice questions, give 5 questions and put the answer key at the end.
Do not claim to know the learner beyond this information."""
            response = client.chat.completions.create(
                model=os.getenv("OPENAI_MODEL", "gpt-4o-mini"),
                messages=[{"role":"system","content":"You create clear, safe, accurate learning materials."},
                          {"role":"user","content":prompt}],
                temperature=0.6,
            )
            return response.choices[0].message.content
        except Exception as e:
            st.warning(f"AI API could not be reached; showing built-in sample content instead. Details: {e}")
    return f"""## {topic}: {level} learning guide

### Learn the idea
Start by describing **{topic}** in your own words. Connect it to something you already know, then study one small idea at a time.

### Key points
- **Core concept:** Identify what {topic} is and the problem it solves.
- **How it works:** Break the process into 3–4 small steps.
- **When to use it:** Look for a practical example from coursework or a project.
- **Common mistake:** Memorizing definitions without testing your understanding.

### Try this
1. Write a one-sentence definition of {topic}.
2. Explain it as if you were teaching a classmate.
3. Create one real-world example and one question you still have.

### Quick recap
Spend 10 minutes reviewing the key points, then complete a short quiz. Your next recommended step is based on your quiz performance.

*Demo mode is active. Add an OPENAI_API_KEY to your environment to generate topic-specific content with an AI model.*"""

def make_quiz(topic, level):
    # Local demo quiz. Replace or supplement with API-generated JSON for production.
    bank = {
        "Beginner": [
            {"q": f"What is the best first step when learning {topic}?", "options":["Memorize everything at once","Understand the core idea","Skip examples","Avoid practice"], "answer":1, "why":"A clear mental model makes later details easier to learn."},
            {"q":"Which activity best checks understanding?", "options":["Re-reading only","Highlighting every line","Explaining the idea in your own words","Skipping review"], "answer":2, "why":"Retrieval and explanation reveal what you really understand."},
            {"q":"What should you do after getting a question wrong?", "options":["Review the explanation and retry","Quit the topic","Guess faster next time","Ignore it"], "answer":0, "why":"Feedback helps you find and correct the knowledge gap."},
        ],
        "Intermediate": [
            {"q":f"Which approach builds stronger understanding of {topic}?", "options":["Apply it to a new example","Only copy a definition","Avoid edge cases","Study without a goal"], "answer":0, "why":"Transfer to new examples tests whether the idea is understood."},
            {"q":"A learner gets similar questions wrong repeatedly. What is the best response?", "options":["Increase difficulty immediately","Identify the misconception and practice it","Remove all feedback","Stop tracking progress"], "answer":1, "why":"Targeted practice is more effective than repeating the same mistake."},
            {"q":"Why is spaced revision useful?", "options":["It removes the need to practice","It supports longer-term recall","It guarantees full marks","It replaces understanding"], "answer":1, "why":"Spacing practice across time can improve retention."},
        ],
        "Advanced": [
            {"q":f"How should you demonstrate mastery of {topic}?", "options":["Repeat a memorized sentence","Apply it to an unfamiliar problem and justify the approach","Avoid explaining assumptions","Use only familiar examples"], "answer":1, "why":"Mastery includes transfer, reasoning, and explaining decisions."},
            {"q":"Which is the strongest sign of a knowledge gap?", "options":["A colorful set of notes","Difficulty explaining why a solution works","A long study session","Many open tabs"], "answer":1, "why":"Explaining reasoning reveals whether knowledge is integrated."},
            {"q":"How should an adaptive system respond to consistently high scores?", "options":["Keep all questions identical","Offer more challenging material","Stop collecting results","Lower the difficulty"], "answer":1, "why":"A suitable challenge helps a learner continue progressing."},
        ],
    }
    return bank[level]

# ------------------------------ Sidebar -------------------------------------
with st.sidebar:
    st.markdown('<div class="brand">✦ <span>AdeptLearn</span></div>', unsafe_allow_html=True)
    st.caption("YOUR PERSONAL AI LEARNING STUDIO")
    st.markdown("---")
    nav_items = [
        ("Overview","⌂"),("AI Study Studio","✦"),("Adaptive Quiz","◎"),
        ("My Progress","▥"),("My Notes","▤")
    ]
    for label, icon in nav_items:
        if st.button(f"{icon}  {label}", key=f"nav_{label}", use_container_width=True,
                     type="primary" if st.session_state.page == label else "secondary"):
            st.session_state.page = label
            st.rerun()
    st.markdown("---")
    st.markdown('<div class="panel"><div class="eyebrow">DAILY FOCUS</div><h3 style="margin:.4rem 0">Small steps.</h3><p class="muted">Build your skills one focused session at a time.</p></div>', unsafe_allow_html=True)
    st.markdown("")
    st.caption("●  Demo workspace")
    st.caption("Settings · Help")

# ------------------------------ Header --------------------------------------
c1, c2 = st.columns([5,1])
with c1:
    st.markdown('<div class="eyebrow">LEARN SMARTER · GROW FASTER</div>', unsafe_allow_html=True)
with c2:
    st.markdown('<div style="text-align:right;color:#AAB5D6;padding-top:4px">✦ AI READY</div>', unsafe_allow_html=True)

page = st.session_state.page

if page == "Overview":
    st.markdown(f"""
    <div class="hero">
      <div class="eyebrow">YOUR LEARNING SPACE</div>
      <h1>Make your next breakthrough. <span style="color:#8EDCFF">✦</span></h1>
      <p>Your personal learning path, adaptive practice, and AI-generated study materials — all in one place.</p>
      <span class="tag">✦ Personalized AI</span><span class="tag">↗ Adaptive learning</span><span class="tag">◷ Learn at your pace</span>
    </div>""", unsafe_allow_html=True)
    st.write("")
    history = st.session_state.quiz_history
    avg = round(sum(x["percent"] for x in history)/len(history)) if history else 0
    m1,m2,m3,m4 = st.columns(4)
    metrics = [(m1,"◈","Learning streak",f'{st.session_state.streak} days',"Keep your rhythm"),(m2,"▣","Study hours",str(st.session_state.study_hours),"This week"),(m3,"◎","Quiz accuracy",f"{avg}%","Based on your attempts"),(m4,"✦","Topics explored",str(len(st.session_state.completed)),"Keep discovering")]
    for col, ico, label, value, foot in metrics:
        with col:
            st.markdown(f'<div class="metric"><div class="metric-label">{ico} &nbsp; {label}</div><div class="metric-value">{value}</div><div class="metric-foot">{foot}</div></div>',unsafe_allow_html=True)
    st.write("")
    st.markdown("### Your AI learning tools")
    f1,f2,f3 = st.columns(3)
    with f1:
        st.markdown('<div class="feature"><div class="feature-icon">✦</div><h3>AI Study Studio</h3><p class="muted">Turn any topic into clear notes, examples, and revision material.</p></div>',unsafe_allow_html=True)
        if st.button("Create study content →",key="go_studio",use_container_width=True):
            st.session_state.page="AI Study Studio"; st.rerun()
    with f2:
        st.markdown('<div class="feature teal"><div class="feature-icon">◎</div><h3>Adaptive Quiz</h3><p class="muted">Check your understanding and let your next level respond to your results.</p></div>',unsafe_allow_html=True)
        if st.button("Start a quick quiz →",key="go_quiz",use_container_width=True):
            st.session_state.page="Adaptive Quiz"; st.rerun()
    with f3:
        st.markdown('<div class="feature pink"><div class="feature-icon">▥</div><h3>Progress insights</h3><p class="muted">See your results, track consistency, and spot what to revise next.</p></div>',unsafe_allow_html=True)
        if st.button("View my progress →",key="go_progress",use_container_width=True):
            st.session_state.page="My Progress"; st.rerun()
    st.write("")
    left,right = st.columns([1.4,1])
    with left:
        st.markdown('<div class="panel"><div class="eyebrow">PICK UP WHERE YOU LEFT OFF</div><h3>Python for AI/ML</h3><p class="muted">Suggested next: functions, parameters, and return values.</p></div>',unsafe_allow_html=True)
        st.progress(0.68, text="Course progress · 68%")
        if st.button("Continue Python learning",key="continue_python"):
            st.session_state.topic="Python functions"; st.session_state.page="AI Study Studio"; st.rerun()
    with right:
        st.markdown('<div class="panel"><div class="eyebrow">RECOMMENDED NEXT</div><h3>Focus on understanding</h3><p class="muted">Generate notes for one topic, then take a short quiz. Review mistakes before moving to a harder level.</p></div>',unsafe_allow_html=True)
        if st.button("Build my study session",key="build_session",use_container_width=True):
            st.session_state.page="AI Study Studio"; st.rerun()

elif page == "AI Study Studio":
    st.markdown("# AI Study Studio")
    st.markdown("Generate personalized learning content for your next study session.")
    with st.container(border=True):
        a,b = st.columns([1.3,1])
        with a:
            topic = st.text_input("What do you want to learn?", value=st.session_state.topic, placeholder="e.g. Python functions, neural networks...")
            kind = st.selectbox("Create",["Study notes","Simple explanation","Practice questions","Revision summary"])
            goals = st.text_area("Your goal (optional)",placeholder="e.g. Prepare for tomorrow's test, understand the basics...",height=90)
        with b:
            level = st.select_slider("Current proficiency",options=["Beginner","Intermediate","Advanced"],value=current_level())
            st.markdown("**Personalization preview**")
            st.markdown(f'<div class="feature blue"><h3>{level} level</h3><p class="muted">Content will be tailored to your selected level and learning goal.</p><span class="tag">Clear explanations</span><span class="tag">Practice-ready</span></div>',unsafe_allow_html=True)
            st.caption("For live AI generation, configure your API key using the setup guide.")
        if st.button("✦ Generate learning content",use_container_width=True,key="generate_content"):
            if not topic.strip():
                st.error("Enter a topic first.")
            else:
                st.session_state.topic=topic.strip()
                st.session_state.level=level
                with st.spinner("Building your personalized learning material..."):
                    st.session_state.generated_content=make_content(topic.strip(),level,kind,goals.strip())
                st.session_state.completed.add(topic.strip())
    if st.session_state.generated_content:
        st.markdown("### Your learning material")
        st.markdown('<div class="panel">',unsafe_allow_html=True)
        st.markdown(st.session_state.generated_content)
        st.markdown('</div>',unsafe_allow_html=True)
        st.download_button("Download notes (.md)",data=st.session_state.generated_content,file_name=f"{st.session_state.topic.lower().replace(' ','_')}_notes.md",mime="text/markdown")
        if st.button("Practice this topic →",key="practice_generated"):
            st.session_state.page="Adaptive Quiz"; st.session_state.quiz=make_quiz(st.session_state.topic,current_level()); st.session_state.quiz_index=0; st.session_state.quiz_answers={}; st.session_state.quiz_finished=False; st.rerun()

elif page == "Adaptive Quiz":
    st.markdown("# Adaptive Quiz")
    st.markdown("Short practice sessions that adjust your suggested level using recent quiz results.")
    top1,top2 = st.columns([2,1])
    with top1:
        quiz_topic = st.text_input("Quiz topic",value=st.session_state.topic,key="quiz_topic_input")
    with top2:
        suggested = current_level()
        st.markdown(f'<div class="metric"><div class="metric-label">Suggested level</div><div class="metric-value" style="font-size:1.35rem">{suggested}</div><div class="metric-foot">Based on recent results</div></div>',unsafe_allow_html=True)
    if not st.session_state.quiz or st.button("↻ New quiz / reset",key="new_quiz"):
        st.session_state.quiz=make_quiz(quiz_topic or "Learning skills",suggested)
        st.session_state.quiz_index=0; st.session_state.quiz_score=0; st.session_state.quiz_answers={}; st.session_state.quiz_finished=False
    quiz = st.session_state.quiz
    if quiz and not st.session_state.quiz_finished:
        idx=st.session_state.quiz_index
        if idx >= len(quiz):
            st.session_state.quiz_finished=True
            st.rerun()
        q=quiz[idx]
        st.progress(idx/len(quiz),text=f"Question {idx+1} of {len(quiz)}")
        st.markdown('<div class="panel">',unsafe_allow_html=True)
        st.markdown(f"### {q['q']}")
        choice=st.radio("Choose one answer",q["options"],key=f"answer_{idx}",index=None)
        if st.button("Check answer →",key=f"check_{idx}"):
            if choice is None:
                st.warning("Choose an answer before continuing.")
            else:
                selected=q["options"].index(choice)
                st.session_state.quiz_answers[idx]={"selected":selected,"correct":selected==q["answer"]}
                if selected==q["answer"]:
                    st.success("Correct! "+q["why"])
                else:
                    st.info("Not quite. "+q["why"])
                st.session_state.quiz_index += 1
                st.rerun()
        st.markdown('</div>',unsafe_allow_html=True)
        answered=st.session_state.quiz_answers.get(idx)
        if answered is not None and st.button("Continue",key=f"continue_{idx}"):
            st.session_state.quiz_index+=1; st.rerun()
    elif st.session_state.quiz_finished or (quiz and st.session_state.quiz_index >= len(quiz)):
        total=len(quiz)
        score=sum(1 for a in st.session_state.quiz_answers.values() if a["correct"])
        pct=round(score/total*100) if total else 0
        st.markdown('<div class="hero">',unsafe_allow_html=True)
        st.markdown(f"## Session complete — {score}/{total} correct")
        st.markdown(f"### {pct}% accuracy")
        st.markdown("Great job showing up to learn. Review the explanation for any missed questions and try again.")
        st.markdown('</div>',unsafe_allow_html=True)
        if not st.session_state.quiz_history or st.session_state.quiz_history[-1].get("session_id") != st.session_state.get("quiz_session_id"):
            st.session_state.quiz_history.append({"date":datetime.now().strftime("%Y-%m-%d %H:%M"),"topic":quiz_topic,"score":score,"total":total,"percent":pct,"level":suggested,"session_id":st.session_state.get("quiz_session_id")})
            st.session_state.quiz_session_id=datetime.now().isoformat()
        if pct>=85:
            st.success("Recommendation: try an advanced topic or a more challenging set next.")
        elif pct>=60:
            st.info("Recommendation: practice a few intermediate examples, then retry.")
        else:
            st.info("Recommendation: revisit the basics and generate beginner notes before another attempt.")
        if st.button("Try another quiz",key="retry_quiz"):
            st.session_state.quiz=make_quiz(quiz_topic,suggested); st.session_state.quiz_index=0; st.session_state.quiz_answers={}; st.session_state.quiz_finished=False; st.session_state.quiz_session_id=None; st.rerun()

elif page == "My Progress":
    st.markdown("# My Progress")
    history=st.session_state.quiz_history
    avg=round(sum(x["percent"] for x in history)/len(history)) if history else 0
    a,b,c=st.columns(3)
    for col,label,value,foot in [(a,"Quiz sessions",len(history),"Sessions completed"),(b,"Average accuracy",f"{avg}%","Across saved attempts"),(c,"Topics explored",len(st.session_state.completed),"Generated study topics")]:
        with col: st.markdown(f'<div class="metric"><div class="metric-label">{label}</div><div class="metric-value">{value}</div><div class="metric-foot">{foot}</div></div>',unsafe_allow_html=True)
    st.write("")
    st.markdown("### Accuracy over time")
    if history:
        import pandas as pd
        df=pd.DataFrame([{"Session":i+1,"Accuracy (%)":x["percent"],"Topic":x["topic"]} for i,x in enumerate(history)])
        st.line_chart(df.set_index("Session")[["Accuracy (%)"]],use_container_width=True)
        st.dataframe(df, use_container_width=True, hide_index=True)
    else:
        st.markdown('<div class="panel"><h3>Your progress starts here</h3><p class="muted">Complete an adaptive quiz to see accuracy trends and learning history here.</p></div>',unsafe_allow_html=True)
    st.markdown("### Study consistency")
    st.progress(min(st.session_state.study_hours/10,1),text=f"{st.session_state.study_hours} of 10 weekly study hours")
    hours=st.slider("Update your study hours this week",min_value=0,max_value=30,value=st.session_state.study_hours)
    st.session_state.study_hours=hours

elif page == "My Notes":
    st.markdown("# My Notes")
    st.markdown("Your most recently generated study material is available here.")
    if st.session_state.generated_content:
        st.markdown(f"### {st.session_state.topic}")
        st.markdown(st.session_state.generated_content)
        st.download_button("Download current notes",data=st.session_state.generated_content,file_name="adeptlearn_notes.md",mime="text/markdown")
    else:
        st.info("No notes yet. Open AI Study Studio and generate your first set of notes.")
        if st.button("Open AI Study Studio"):
            st.session_state.page="AI Study Studio"; st.rerun()

st.markdown("---")
st.markdown('<div style="text-align:center;color:#7783A8;font-size:.78rem">ADEPTLEARN · Learn at your pace. Grow with purpose. · Prototype build</div>',unsafe_allow_html=True)
