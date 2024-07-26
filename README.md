import streamlit as st
import google.generativeai as genai

def generate_craft_idea(materials, item, skill_level):
    """Generates an art and craft idea and drawing instructions based on the given materials, item, and skill level.

    Args:
        materials: The materials available for the craft.
        item: The item to draw.
        skill_level: The skill level for the drawing instructions.

    Returns:
        The generated art and craft idea and drawing instructions.
    """

    try:
        # Configure API key
        genai.configure(api_key="AIzaSyBZKWOxPAaF357psP9I2hYToZIW6kzqi_0")

        # Create the model
        generation_config = {
            "temperature": 1,
            "top_p": 0.95,
            "top_k": 64,
            "max_output_tokens": 8192,
            "response_mime_type": "text/plain",
        }
        model = genai.GenerativeModel(
            model_name="gemini-1.5-flash",
            generation_config=generation_config,
            system_instruction="You are an art and craft expert. Create engaging and creative art and craft ideas and provide detailed drawing instructions using the given materials.",
        )

        # Construct the prompt
        prompt = f"Create an engaging art and craft project and provide detailed instructions for drawing a {item} using the following materials: {materials}. The project should be creative and feasible with the provided items. The instructions should be suitable for a {skill_level} artist."

        # Generate the art and craft idea and drawing instructions
        response = model.generate_content(prompt)
        return response.text
    except Exception as e:
        return f"Error: {e}"

# Streamlit application
st.set_page_config(page_title="Art and Craft Idea Generator", page_icon=":art:")

st.title("Art and Craft Idea Generator :art:")

st.write("""
    Welcome to the Art and Craft Idea Generator! This tool helps you create engaging and creative art and craft projects 
    using the materials you have available. You can also get detailed instructions on how to draw a specific item.
""")

materials = st.text_input("Enter the available materials:", placeholder="e.g., paint, pencils, erasers, glue")
item = st.text_input("Enter the item name:", placeholder="e.g., cat, tree, car")
skill_level = st.selectbox("Select the skill level:", ("beginner", "intermediate", "advanced"))

if st.button("Generate Art and Craft Idea and Drawing Instructions"):
    if materials and item and skill_level:
        with st.spinner("Generating your art and craft idea and drawing instructions..."):
            result = generate_craft_idea(materials, item, skill_level)
        st.subheader("Generated Art and Craft Idea and Drawing Instructions:")
        st.write(result)
    else:
        st.error("Please enter the available materials, item name, and select a skill level.")

st.markdown("""
    ---
    Need help? Contact our support team or visit our [documentation](https://www.example.com/docs).
""")
