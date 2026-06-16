# rtcbook-web

The public web site for **rtcbook.org** — the partial, openly-licensed web
edition of *Real-Time Computing for Mechanical Engineers*. A thin Django
project on top of the [`parody-web`](https://github.com/ricopicone/parody-web)
package; it imports the book's parody artifact and renders it.

- **Public** visitors see only the openly-licensed (`online_only`) sections.
- **Owner** (the only account) signs in at `/accounts/login/` to read the whole
  book. The full text is gated; MIT Press licensing keeps it off the open web.

The renderer is the `parody-web` package (pinned in `requirements.txt`); this
repo is just configuration + deploy. Content is pulled at deploy from the
book's content repo release (`ricopicone/real-time-computing-parody`).

## Local

```bash
python -m venv .venv && . .venv/bin/activate && pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
# build the artifact in the content repo, then:
python manage.py import_artifact /path/to/real-time-computing.json --slug real-time-computing
python manage.py runserver
```

## Deploy (AWS via SSM)

See [`deploy/AWS.md`](deploy/AWS.md). This repo's `deploy.yml` calls the shared
reusable workflow in `parody-web`; set repo variables `AWS_REGION`,
`INSTANCE_ID`, `APP_DIR` and secret `AWS_DEPLOY_ROLE_ARN`.
