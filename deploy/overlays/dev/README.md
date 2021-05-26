### Aanpassen van image tag

In Kustomize file

    images:
    - name: gbaeke/flux-rt
      newTag: "1234"

Command

    kustomize edit set image gbaeke/flux-rt:1234

- Kustomize edit wijzigt tag van genoemde image
- Als image name niet bestaat wordt deze toegevoegd
- De image tag vervangt de image tag in de kustomized deployment file

Op Gthub Actions (zie https://github.com/marketplace/actions/setup-kustomize)

    on:
        push:
            branches:
              - master
    jobs:
        create-deployment-branch:
            runs-on: ubuntu-latest
            needs:
                - publish-image
            steps:
                - uses: imranismail/setup-kustomize@v1
                - run: |
                    kustomize edit set image app:${GITHUB_SHA}
                    git add .
                    git commit -m "Set `app` image tag to `${GITHUB_SHA}`"
                    git push
