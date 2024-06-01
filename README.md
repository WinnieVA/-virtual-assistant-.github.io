import requests
from bs4 import BeautifulSoup

def fetch_profile(url):
    response = requests.get(url)
    return response.text

def parse_profile(profile_html):
    soup = BeautifulSoup(profile_html, 'html.parser')
    profile_data = {
        'headline': soup.find('h1', {'class': 'profile-title'}).text.strip(),
        'skills': [skill.text for skill in soup.find_all('span', {'class': 'skill-name'})],
        'feedback': soup.find('div', {'class': 'feedback-score'}).text.strip(),
        'job_success': soup.find('div', {'class': 'job-success-score'}).text.strip(),
    }
    return profile_data

def compare_profiles(profile1, profile2):
    differences = {}
    for key in profile1:
        if profile1[key] != profile2[key]:
            differences[key] = {'profile1': profile1[key], 'profile2': profile2[key]}
    return differences

profile1_url = '[https://www.upwork.com/freelancers/PROFILE1_ID](https://www.upwork.com/nx/search/talent/?nav_dir=pop&nbs=1&q=virtual%20assistant%20entry%20level)'
profile2_url = '[https://www.upwork.com/freelancers/PROFILE2_I](https://www.upwork.com/nx/find-work/)'

profile1_html = fetch_profile(profile1_url)
profile2_html = fetch_profile(profile2_url)

profile1_data = parse_profile(profile1_html)
profile2_data = parse_profile(profile2_html)

differences = compare_profiles(profile1_data, profile2_data)
print(differences)
