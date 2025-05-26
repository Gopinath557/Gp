#include <stdio.h>
#include <string.h>

void search(char pat[], char txt[]) {
    int n = strlen(txt);
    int m = strlen(pat);

    // Loop through the text
    for (int i = 0; i <= n - m; i++) {
        // Check for the pattern
        int j;
        for (j = 0; j < m; j++) {
            if (txt[i + j] != pat[j]) {
                break;
            }
        }
        // If the pattern is found
        if (j == m) {
            printf("Pattern found at index %d\n", i);
        }
    }
}

int main() {
    char txt[] = "ABABDABACDABABCABAB";
    char pat[] = "ABABCABAB";
    search(pat, txt);
    return 0;
}
