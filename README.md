import requests
import base64
import whatismyip


def update_ip():
  githubAPIURL = "https://api.github.com/repos/NkosSmyrnakis/door_ip/contents/README.md"
  githubToken = "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"
  with open('README.md', 'w') as f:
      f.write(whatismyip.whatismyip())
  with open("README.md", "rb") as f:
      encodedData = base64.b64encode(f.read())

      headers = {
          "Authorization": f"Bearer {githubToken}",
          "Content-type": "application/vnd.github+json"
      }

      # Καλούμε το GitHub API για να ανακτήσουμε τα δεδομένα του υπάρχοντος αρχείου
      r = requests.get(githubAPIURL, headers=headers)
      response = r.json()

      # Ενημερώνουμε τα δεδομένα με το νέο περιεχόμενο
      data = {
          "message": "Το μήνυμα μου για την ενημέρωση",
          "content": encodedData.decode("utf-8"),
          "sha": response["sha"]  # Περνάμε το sha του υπάρχοντος αρχείου
      }

      # Καλούμε το GitHub API για να ενημερώσουμε το αρχείο
      r = requests.put(githubAPIURL, headers=headers, json=data)
      #print(r.text)


update_ip()
