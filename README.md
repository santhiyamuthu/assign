# assign
import os
import requests
from langchain.llms import OpenAI
from bs4 import BeautifulSoup


llm = OpenAI(api_key='_____')


class ResearchAgent:
    """Finds trending HR topics and relevant information."""

    def fetch_trending_topics(self):
        url = "https://www.hr.com/en/app/blogs/"
        headers = {'User-Agent': 'Mozilla/5.0'}
        response = requests.get(url, headers=headers)
        soup = BeautifulSoup(response.text, 'html.parser')
        topics = [h2.get_text(strip=True) for h2 in soup.find_all('h2')][:5]
        return topics if topics else ["The Future of Remote Work", "AI in HR", "Employee Well-being"]


class ContentPlanningAgent:
    """Creates a structured outline for the blog."""

    def generate_outline(self, topic):
        prompt = f"Generate a unique, engaging, and SEO-optimized blog outline on {topic}. Consider reader engagement and structuring for clarity."
        return llm(prompt)


class ContentGenerationAgent:
    """Writes blog content based on the outline."""

    def generate_content(self, outline):
        prompt = f"Write a compelling, well-researched 2000-word blog post using the following outline:\n{outline}. Ensure a conversational yet professional tone."
        return llm(prompt)


class SEOOptimizationAgent:
    """Ensures SEO best practices are followed."""

    def optimize_content(self, content):
        prompt = f"Optimize this blog for SEO by incorporating relevant keywords, improving readability, and enhancing engagement:\n{content}"
        return llm(prompt)


class ReviewAgent:
    """Proofreads and improves content quality."""

    def review_content(self, content):
        prompt = f"Proofread and refine the following blog for coherence, grammar, and readability while maintaining a natural flow:\n{content}"
        return llm(prompt)



def generate_blog():
    research_agent = ResearchAgent()
    content_planner = ContentPlanningAgent()
    content_writer = ContentGenerationAgent()
    seo_agent = SEOOptimizationAgent()
    review_agent = ReviewAgent()

    trending_topics = research_agent.fetch_trending_topics()
    print("Trending Topics:", trending_topics)
    selected_topic = trending_topics[0]

    outline = content_planner.generate_outline(selected_topic)
    content = content_writer.generate_content(outline)
    optimized_content = seo_agent.optimize_content(content)
    final_content = review_agent.review_content(optimized_content)

    output_filename = f"blog_{selected_topic.replace(' ', '_').lower()}.md"
    with open(output_filename, "w", encoding="utf-8") as file:
        file.write(final_content)
    print(f"Blog saved as {output_filename}")


if __name__ == "__main__":
    generate_blog()
